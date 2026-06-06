# Mon blog — Jekyll + GitHub Pages

Blog statique au style sobre, hébergé **gratuitement** sur GitHub Pages.
GitHub construit le site automatiquement à chaque fois que tu pousses du
contenu : pas de serveur à gérer, pas d'outil à installer obligatoirement.

---

## 🚀 Mise en ligne (une seule fois)

1. **Crée un dépôt GitHub** nommé **exactement** :
   ```
   ton-pseudo.github.io
   ```
   (remplace `ton-pseudo` par ton vrai identifiant GitHub — c'est ce nom qui
   donne l'URL de ton blog).

2. **Pousse ces fichiers** dans le dépôt :
   ```bash
   cd mon-blog-jekyll
   git init
   git add .
   git commit -m "Mon blog"
   git branch -M main
   git remote add origin https://github.com/ton-pseudo/ton-pseudo.github.io.git
   git push -u origin main
   ```

3. Sur GitHub : **Settings → Pages → Build and deployment → Source = «Deploy
   from a branch»**, branche `main`, dossier `/ (root)`. Enregistre.

4. Attends ~1 minute. Ton blog est en ligne sur :
   ```
   https://ton-pseudo.github.io
   ```

5. **Dernier réglage** : ouvre `_config.yml` et décommente la ligne `url:` en y
   mettant `https://ton-pseudo.github.io` (important pour le flux RSS et le SEO).

---

## ✍️ Écrire un article — deux façons

### Option A — depuis ton navigateur (zéro installation, ton « dashboard »)

C'est le plus simple pour commencer :

1. Sur GitHub, va dans le dossier `_posts/`.
2. Clique **Add file → Create new file**.
3. Nomme-le `AAAA-MM-JJ-titre-de-larticle.md` (ex. `2026-07-01-mon-article.md`).
   ⚠️ La date dans le nom du fichier est **obligatoire** et doit respecter ce format.
4. Colle l'en-tête puis écris :
   ```markdown
   ---
   layout: post
   title: "Le titre de mon article"
   description: "Une phrase de résumé."
   date: 2026-07-01
   ---

   Mon contenu en Markdown commence ici.
   ```
5. Clique **Commit changes**. Une minute plus tard, l'article est en ligne.

### Option B — en local (si tu veux prévisualiser avant de publier)

Nécessite **Ruby** (souvent déjà présent sur Linux ; sinon
`sudo apt install ruby-full build-essential`) :

```bash
bundle install            # une seule fois
bundle exec jekyll serve  # → http://localhost:4000
```

Tu écris tes fichiers `.md` dans `_posts/`, la page se recharge automatiquement.
Quand tu es content, `git push` publie.

---

## ✦ Aide-mémoire Markdown

```markdown
## Un titre de section
### Un sous-titre

Du texte normal, avec du **gras** et de l'*italique*.

- une puce
- une autre

> une citation

`du code en ligne`

​```python
# un bloc de code (coloré automatiquement)
print("bonjour")
​```

| Colonne A | Colonne B |
|---|---|
| valeur | valeur |

[un lien](https://exemple.com)
![une image](/assets/mon-image.png)
```

Pour ajouter une image : dépose-la dans `assets/` et référence-la par
`/assets/nom-de-limage.png`.

---

## 🎨 Personnaliser

- **Titre, sous-titre, bio courte, liens** → `_config.yml`
- **Texte de la page d'accueil** → `index.html`
- **Page « À propos »** → `about.md`
- **Couleurs, polices, espacements** → `assets/css/style.css`
  (tout est en variables CSS en haut, section `:root` — la couleur d'accent est
  `--accent`)

---

## 📁 Structure

```
.
├── _config.yml          ← config du site (édite ici en premier)
├── index.html           ← page d'accueil
├── blog.html            ← liste de tous les articles
├── about.md             ← page « À propos »
├── _posts/              ← TES ARTICLES (.md, nommés AAAA-MM-JJ-titre.md)
├── _layouts/            ← gabarits (base, article, page)
├── _includes/           ← en-tête, pied de page, date
├── assets/
│   ├── css/style.css    ← tout le design
│   └── favicon.svg
└── Gemfile              ← pour la prévisualisation locale uniquement
```

---

## ℹ️ Note

Ce site n'utilise que des extensions supportées nativement par GitHub Pages
(`jekyll-feed`, `jekyll-seo-tag`, `jekyll-sitemap`) : GitHub le construit donc
sans configuration supplémentaire. Le flux RSS est disponible à `/feed.xml` et
un plan de site à `/sitemap.xml`.

Bon blog. ✦
