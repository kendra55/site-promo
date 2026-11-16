# Promo CDA

Le site de présentation de la promo : une page d'accueil qui liste les membres, et **une carte HTML par personne**.
Il est construit à plusieurs, avec Git et GitHub : chaque changement arrive par une Pull Request relue par un pair.

## Voir le site

Pas d'installation : ouvrez `index.html` dans votre navigateur (`xdg-open index.html` sous Linux, `open index.html`
sous macOS, `explorer.exe index.html` sous WSL).

## Structure

```
.
├── index.html                     # page d'accueil : le titre et la liste <ul class="promo"> des membres
├── style.css                      # styles communs (variables CSS en haut du fichier)
├── cartes/
│   └── exemple.html               # modèle de carte : à copier en cartes/<prenom>.html
├── CONTRIBUTING.md                # règles de contribution : branches, commits, PR, revue
├── .githooks/
│   └── commit-msg                 # vérifie Conventional Commits (git config core.hooksPath .githooks)
└── .github/
    ├── pull_request_template.md   # texte pré-rempli de chaque Pull Request
    └── CODEOWNERS                 # (ajouté pendant le TP) qui doit relire quoi
```

## Démarrer

```bash
git config core.hooksPath .githooks   # active la vérification des messages de commit
```

Puis suivez [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Règles de la branche `main`

`main` est protégée par un **ruleset** : personne n'y pousse directement, pas même les administrateurs. Tout passe par
une Pull Request approuvée par au moins une autre personne (dont un propriétaire du code concerné, voir `CODEOWNERS`),
toutes les conversations résolues, puis fusionnée en **squash**.
