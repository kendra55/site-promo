# Contribuer au site « Promo CDA »

Ces règles sont courtes : elles permettent à plusieurs personnes de travailler en même temps sans se marcher dessus, et
à n'importe qui de comprendre l'historique dans six mois.

## 1. Le principe : GitHub Flow

- `main` contient toujours un site qui fonctionne. **Personne ne pousse directement sur `main`** (le ruleset le refuse).
- Chaque changement se fait sur **sa propre branche**, courte (quelques heures), partie de la dernière version de
  `main`, et revient dans `main` par une **Pull Request** relue puis fusionnée en **squash**.

```
main     ──●──────────────●──────────●──▶      (un commit par PR, grâce au squash)
            \            ▲          ▲
feature/…    ●───●──────●   PR relue, conversations résolues, squash and merge
```

## 2. Nommer sa branche

Format : `<type>/<sujet-court>`, en minuscules, sans accent ni espace.

| Type | Quand | Exemple |
|---|---|---|
| `feature/` | Ajout de contenu ou de fonctionnalité | `feature/carte-alice` |
| `fix/` | Correction | `fix/lien-casse-accueil` |
| `docs/` | Documentation seulement | `docs/codeowners` |
| `chore/` | Configuration du dépôt | `chore/codeowners` |

```bash
git switch main && git pull
git switch -c feature/carte-<prenom>
```

## 3. Messages de commit : Conventional Commits

```
<type>(<portée optionnelle>): <description à l'impératif, en minuscules, sans point final>
```

| Type | Usage | Exemple |
|---|---|---|
| `feat` | Nouveau contenu | `feat(cartes): ajoute la carte d'Alice` |
| `fix` | Correction | `fix(accueil): corrige le lien vers la carte de Bob` |
| `style` | CSS, mise en forme | `style(cartes): change la couleur de l'avatar d'Alice` |
| `docs` | Documentation | `docs: précise la convention de nommage` |
| `refactor` | Réorganisation sans changement visible | `refactor(css): regroupe les variables` |
| `chore` | Configuration du dépôt | `chore: ajoute CODEOWNERS` |

- **Un commit = une idée.** La description dit ce que **fait** le commit (`ajoute`, pas `ajouté`).
- Le hook `.githooks/commit-msg` refuse les messages non conformes : activez-le avec
  `git config core.hooksPath .githooks`.
- Le **titre de la PR** suit le même format : en *squash and merge*, c'est lui qui devient le message dans `main`.

## 4. Ajouter sa carte

1. `cp cartes/exemple.html cartes/<prenom>.html`, puis personnalisez titre, initiale, nom, rubriques, étiquettes.
   Restez bienveillant : le dépôt est public.
2. Ajoutez **une seule ligne** dans la liste `<ul class="promo">` de `index.html`, juste au-dessus du commentaire :
   `<li><a href="cartes/<prenom>.html">Prénom</a></li>`
3. Vérifiez dans le navigateur : la carte s'affiche, les liens « aller » et « retour » fonctionnent.

## 5. Ouvrir une Pull Request

1. `git push -u origin <ma-branche>`
2. Sur GitHub, **Compare & pull request**. Titre conventionnel ; complétez **toutes** les rubriques du modèle.
3. GitHub ajoute automatiquement les relecteurs définis dans `.github/CODEOWNERS` ; ajoutez-en un si besoin.
4. Pas prête ? Ouvrez-la en **Draft**, passez-la en *Ready for review* ensuite.

## 6. Relire une Pull Request : *conventional comments*

Onglet **Files changed**. Vérifiez dans cet ordre : fait-elle ce qu'annonce la description ? est-ce correct (liens,
casse des noms de fichiers) ? est-ce lisible ? le titre et la branche respectent-ils ce guide ? un fichier en trop ?

Chaque commentaire commence par un **label** qui annonce son intention :

| Label | Sens |
|---|---|
| `praise:` | Ce qui est réussi (au moins un par revue) |
| `issue (blocking):` | À corriger avant le merge |
| `suggestion (non-blocking):` | Proposition, l'auteur décide |
| `question:` | Demande d'explication, pas un reproche |
| `nitpick:` | Détail, jamais bloquant |
| `todo:` | Petite tâche nécessaire |

Pour proposer directement un changement, utilisez le bouton **Add a suggestion** (bloc ` ```suggestion `) : l'auteur
l'applique en un clic (*Commit suggestion*).

| À éviter | À préférer |
|---|---|
| « C'est faux. » | `issue (blocking): le lien pointe vers cartes/Alice.html, mais le fichier est alice.html (la casse compte sous Linux).` |
| « Pas terrible le CSS. » | `suggestion (non-blocking): var(--color-accent) plutôt que #2f5bea, pour rester cohérent ?` |

Puis **Review changes** : *Comment*, *Request changes* (au moins un `issue (blocking)`) ou *Approve*. L'auteur répond à
**chaque** conversation et la marque *Resolve conversation* : le ruleset l'exige avant le merge.

## 7. Fusionner

Quand la PR est approuvée, à jour, sans conflit et sans conversation ouverte : **Squash and merge** (vérifiez que le
titre proposé est conventionnel), puis **Delete branch**. En local :

```bash
git switch main && git pull
git branch -D <ma-branche>      # -D : après un squash, Git ne voit pas la branche comme « fusionnée »
```

## 8. En cas de conflit

GitHub affiche *This branch has conflicts that must be resolved*. Résolvez **en local**, sur votre branche :

```bash
git switch main && git pull
git switch <ma-branche>
git merge main                  # Git signale le conflit
# éditez le fichier, supprimez les marqueurs <<<<<<< ||||||| ======= >>>>>>>
git diff --check                # ne doit rien signaler
git add <fichier> && git commit --no-edit
git push
```

- Conflit **textuel** (deux lignes ajoutées au même endroit) : gardez les deux.
- Conflit **sémantique** (deux intentions différentes sur la même ligne) : **parlez à l'autre auteur** avant de choisir,
  et indiquez la décision dans la PR.
- Après la résolution, le relecteur doit **re-approuver** (les approbations périmées sont annulées par le ruleset).
