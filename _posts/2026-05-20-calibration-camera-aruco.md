---
layout: post
title: "Calibrer une caméra avec des marqueurs ArUco"
description: "Passer des pixels aux coordonnées du monde réel, sans matériel de calibration coûteux — juste du carton imprimé et un peu de géométrie."
date: 2026-05-20
---

Sur un système de contrôle d'accès, la question n'est pas seulement *« est-ce qu'il y a une personne ? »* mais *« où est-elle, en mètres, dans la pièce ? »*. Une détection YOLO me donne une boîte en pixels ; il me faut une transformation vers le plan réel. Les marqueurs ArUco rendent ça presque trivial.

## L'idée

Un marqueur ArUco est un motif noir et blanc dont OpenCV connaît la taille physique. En plaçant quelques marqueurs à des positions connues du sol, j'obtiens une correspondance entre pixels et coordonnées réelles. C'est une homographie — une matrice 3×3 qui projette un plan sur un autre.

## Le code

D'abord, détecter les marqueurs dans l'image :

```python
import cv2
import numpy as np

aruco_dict = cv2.aruco.getPredefinedDictionary(cv2.aruco.DICT_4X4_50)
params = cv2.aruco.DetectorParameters()
detector = cv2.aruco.ArucoDetector(aruco_dict, params)

frame = cv2.imread("scene.jpg")
corners, ids, _ = detector.detectMarkers(frame)
```

Ensuite, on associe chaque marqueur détecté (en pixels) à sa position réelle connue (en mètres), puis on calcule l'homographie :

```python
# Coins image (pixels) -> coins monde (mètres)
image_pts = np.array([c[0].mean(axis=0) for c in corners], dtype=np.float32)
world_pts = np.array([
    [0.0, 0.0],   # marqueur id 0 au coin de la pièce
    [3.0, 0.0],   # 3 m sur l'axe X
    [3.0, 4.0],
    [0.0, 4.0],
], dtype=np.float32)

H, _ = cv2.findHomography(image_pts, world_pts)
```

Enfin, projeter n'importe quel point de l'image vers le monde réel :

```python
def pixel_to_world(px, py, H):
    pt = np.array([px, py, 1.0])
    world = H @ pt
    world /= world[2]          # normalisation homogène
    return world[0], world[1]  # x, y en mètres
```

## Précision en pratique

Sur mon installation, voici l'erreur médiane mesurée selon la distance à la caméra :

| Distance | Erreur médiane | Note |
|---|---|---|
| < 2 m | ~4 cm | excellent |
| 2–4 m | ~9 cm | acceptable |
| > 4 m | ~22 cm | à recouper avec le RFID |

La leçon : **un seul capteur ne suffit jamais**. Au-delà de quatre mètres, je fusionne la position visuelle avec celle des balises RFID. Mais ça, ce sera le sujet d'un prochain article.

## À retenir

- Imprime tes marqueurs en grand : un ArUco minuscule à 4 m, c'est du bruit.
- Mesure les positions réelles au cordeau, pas à l'œil.
- Garde toujours un capteur de secours pour les zones lointaines.
