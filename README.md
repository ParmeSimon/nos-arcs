# Nos Arcs — carnet de campagne coop

Une page web pour suivre les jeux que vous enchaînez à deux : arc en cours, file
d'attente, jeux terminés et recommandations basées sur vos genres.

**Contrainte de départ :** ton portfolio est déjà en ligne sur `simonparme.com`,
via un autre dépôt GitHub Pages. On ne touche **ni à ce dépôt, ni aux
enregistrements DNS existants**. Résultat : ce carnet vit dans un **nouveau
dépôt séparé**, servi sur le sous-domaine **`arc.simonparme.com`**.

C'est un enregistrement DNS **en plus**, pas une modification de ceux qui font
déjà marcher ton portfolio.

---

## Étape 1 — Créer un nouveau dépôt (différent de celui du portfolio)

Sur github.com : **New repository**. Un nom au choix (ex. `nos-arcs`),
**visibilité Public**, aucune case d'initialisation cochée (tu as déjà les
fichiers). ⚠️ Vérifie bien que ce n'est **pas** le dépôt de ton portfolio.

## Étape 2 — Pousser le code

Dans le dossier qui contient `index.html`, `games.json` et `CNAME` :

```bash
git init
git add .
git commit -m "Nos Arcs — carnet coop"
git branch -M main
git remote add origin https://github.com/parmesimon/nos-arcs.git
git push -u origin main
```

Remplace `nos-arcs` par le nom réel que tu as donné au dépôt à l'étape 1.

## Étape 3 — Activer GitHub Pages (sur ce nouveau dépôt uniquement)

1. Sur **ce** dépôt (pas celui du portfolio) : **Settings → Pages**.
2. *Source* : **Deploy from a branch** → **main** / **(root)** → **Save**.
3. Vérifie que `https://parmesimon.github.io/nos-arcs/` affiche le carnet avant
   de continuer.

## Étape 4 — Brancher arc.simonparme.com

**A. Sur ce nouveau dépôt** — *Settings → Pages → Custom domain* : tape
`arc.simonparme.com`, **Save**. (Le fichier `CNAME` que tu as poussé contient
déjà cette valeur, donc ce champ devrait se pré-remplir tout seul.)

**B. Chez OVH** — *Noms de domaine → simonparme.com → Zone DNS* : **ajoute**
(sans toucher aux autres) un enregistrement :

```
Type   Sous-domaine   Cible
CNAME  arc            parmesimon.github.io.
```

C'est tout. Aucun enregistrement existant à supprimer ou modifier — celui-ci
s'ajoute simplement à côté de ceux de ton portfolio.

Compte de quelques minutes à ~1 h de propagation. Une fois propagé, reviens sur
**Settings → Pages** du nouveau dépôt : le message doit passer au vert (domaine
vérifié), puis coche **Enforce HTTPS**.

## Étape 5 — Relier le bouton « Enregistrer » à ton dépôt

Ouvre `index.html`, tout en bas, et vérifie/ajuste :

```html
<script>
window.REPO = { owner: "parmesimon", name: "nos-arcs", branch: "main" };
window.DATA_FILE = "games.json";
</script>
```

Adapte `name` si tu as choisi un nom de dépôt différent de `nos-arcs`.

---

## Enregistrement automatique (recommandé)

Chaque modification peut être commitée directement dans `games.json`, sans
copier-coller. Il faut un token GitHub, stocké uniquement dans le navigateur
(jamais dans le dépôt).

1. Bas de page → **Activer l'enregistrement automatique**.
2. Crée un token sur <https://github.com/settings/personal-access-tokens/new> :
   *Fine-grained*, **Only select repositories** → ce dépôt,
   **Permissions → Contents : Read and write**.
3. Colle-le, **Connecter**.

Ensuite : chaque action est commitée ~1 s après, et la page récupère toute
seule les modifs de l'autre joueur (toutes les 20 s, et au retour sur l'onglet).

**Ton pote** : un token fine-grained ne peut viser que les dépôts de son
propriétaire. Soit tu l'ajoutes en collaborateur (*Settings → Collaborators*)
et il crée un token *classic* avec le scope `public_repo`, soit tu crées un
second token fine-grained pour lui. Chaque token se révoque à tout moment
depuis les réglages GitHub.

Sans token, l'ancien mode manuel ci-dessous reste disponible.

## Utilisation au quotidien (mode manuel)

1. Sur `arc.simonparme.com`, ajoute un jeu, marque-le terminé, note-le,
   réordonne la file… tout se met à jour à l'écran immédiatement.
2. Une **barre en bas** apparaît dès qu'il y a des changements non enregistrés :
   clique **« Enregistrer… »**.
3. Une fenêtre s'ouvre avec le contenu à jour de `games.json`. Clique
   **Copier**, puis **Ouvrir l'éditeur GitHub** — ça t'amène direct sur la page
   d'édition de `games.json` sur GitHub. Sélectionne tout (Ctrl/Cmd+A), colle,
   puis **Commit changes**.
4. Reviens sur la fenêtre et clique **« C'est commité »**.
5. GitHub Pages redéploie en général en **moins d'une minute**.

### Deux personnes qui modifient en même temps ?
Comme pour n'importe quel fichier Git : le second à committer peut voir un
conflit si vous avez touché les deux au même moment. Pour un carnet mis à jour
de temps en temps, ça n'arrive presque jamais.

---

## Bon à savoir

- **`games.json` absent** (premier déploiement) : la page charge automatiquement
  le contenu de départ (Satisfactory en cours, Valheim / Scrap Mechanic / Albion
  en file) et t'invite tout de suite à l'enregistrer pour créer le fichier.
- **Le portfolio n'est jamais concerné** : dépôt différent, enregistrements DNS
  différents (`arc.` en plus, `@` inchangé).
- **Thème clair/sombre** : la page suit le réglage de ton système.
- Aucune dépendance à installer, aucun build, aucune clé API, aucun compte tiers
  — juste GitHub Pages (gratuit) et un enregistrement DNS de plus chez OVH.
