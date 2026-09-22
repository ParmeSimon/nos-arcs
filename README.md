# Nos Arcs — carnet de campagne coop

Une page web pour suivre les jeux que vous enchaînez à deux : arc en cours, file
d'attente, jeux terminés et recommandations basées sur vos genres.

**Comment ça marche :** toutes les données vivent dans **`games.json`**, un fichier
de ton dépôt. La page le lit au chargement. Quand tu ajoutes/coches/notes un jeu
dans l'interface, ça se voit tout de suite à l'écran — mais pour que ce soit
**définitif et visible par ton pote**, il faut enregistrer : la page te génère le
contenu à jour et te propose de l'ouvrir dans l'éditeur GitHub pour committer.

Pas de service externe, pas de compte à créer, pas de clé à gérer. Juste du Git.

---

## Les fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | Toute la page (design + logique). |
| `games.json` | Vos données. C'est le seul fichier que vous modifiez au fil du temps. |
| `CNAME` | Domaine personnalisé pour GitHub Pages. |

## Étape 1 — Mettre le code sur GitHub

Depuis le dossier qui contient ces fichiers :

```bash
git init
git add .
git commit -m "Nos Arcs — carnet coop"
git branch -M main
# Crée d'abord un dépôt vide sur github.com (ex. "nos-arcs"), puis :
git remote add origin https://github.com/TON-PSEUDO/nos-arcs.git
git push -u origin main
```

Remplace `TON-PSEUDO` par ton pseudo GitHub et `nos-arcs` par le nom de ton dépôt.
Le dépôt doit être **public** pour GitHub Pages gratuit (et pour que l'éditeur en
ligne fonctionne sans droits spéciaux pour ton pote — voir plus bas).

> C'est cette étape (et la suivante) qui nécessite **ton** compte : je ne peux pas
> pousser à ta place, ni toucher à tes identifiants.

## Étape 2 — Activer GitHub Pages

1. Sur GitHub : **Settings → Pages**.
2. *Source* : **Deploy from a branch**.
3. *Branch* : **main**, dossier **/ (root)** → **Save**.
4. Au bout d'une minute, ton site est en ligne sur `https://TON-PSEUDO.github.io/nos-arcs/`.
   Vérifie que ça marche là **avant** de brancher le domaine.

## Étape 3 — Brancher simonparme.com

⚠️ **À lire d'abord.** Le fichier `CNAME` est réglé sur l'apex **`simonparme.com`**.
Si tu as **déjà un site** sur `simonparme.com`, ne l'écrase pas : utilise plutôt un
sous-domaine (voir l'encadré plus bas).

**A. Côté GitHub** — *Settings → Pages → Custom domain* : tape `simonparme.com`, **Save**.
Coche **Enforce HTTPS** une fois le certificat prêt (peut prendre quelques minutes).

**B. Côté DNS** (chez ton registrar). Quatre enregistrements **A** sur l'apex `@` :

```
Type  Hôte   Valeur
A     @      185.199.108.153
A     @      185.199.109.153
A     @      185.199.110.153
A     @      185.199.111.153
```

Recommandé en plus, pour l'IPv6, quatre enregistrements **AAAA** sur `@` :

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

Et un **CNAME** pour `www` (redirige vers l'apex) :

```
Type   Hôte   Valeur
CNAME  www    TON-PSEUDO.github.io.
```

Compte de quelques minutes à ~1 h de propagation DNS.

> **Variante sous-domaine (recommandée si l'apex est déjà pris).**
> Remplace le contenu du fichier `CNAME` par `arcs.simonparme.com`, mets ce même
> `arcs.simonparme.com` dans *Settings → Pages → Custom domain*, et côté DNS crée un
> **seul** enregistrement : `CNAME  arcs  TON-PSEUDO.github.io.` — pas besoin des A/AAAA.
> Ton site principal sur `simonparme.com` n'est pas touché.

## Étape 4 — Relier le bouton « Enregistrer » à ton dépôt

Ouvre `index.html`, tout en bas, et renseigne ton dépôt :

```html
<script>
window.REPO = { owner: "TON-PSEUDO", name: "nos-arcs", branch: "main" };
window.DATA_FILE = "games.json";
</script>
```

Avec ça, le bouton **« Ouvrir l'éditeur GitHub »** de la fenêtre d'enregistrement
t'amène directement sur la page d'édition du bon fichier, sur la bonne branche.

---

## Utilisation au quotidien

1. Sur la page, ajoute un jeu, marque-le terminé, note-le, réordonne la file… tout
   se met à jour à l'écran immédiatement.
2. Une **barre en bas** apparaît dès qu'il y a des changements non enregistrés :
   clique **« Enregistrer… »**.
3. Une fenêtre s'ouvre avec le contenu à jour de `games.json` (et, si tu préfères,
   un onglet `arcs.md` — une version lisible, à créer si tu veux une jolie page
   markdown sur GitHub, optionnelle).
4. Clique **Copier**, puis **Ouvrir l'éditeur GitHub** — ça t'amène direct sur la
   page d'édition de `games.json` sur GitHub. Sélectionne tout (Ctrl/Cmd+A), colle,
   puis en bas de la page GitHub clique **Commit changes**.
5. Reviens sur la fenêtre et clique **« C'est commité »**.
6. GitHub Pages redéploie en général en **moins d'une minute**. Ton pote (et toi,
   après un rechargement) verra la mise à jour.

Pas envie d'ouvrir GitHub tout de suite ? Le bouton **Télécharger** te donne le
fichier en local, à uploader plus tard par glisser-déposer sur GitHub.

### Et si je n'ai pas encore lié `window.REPO` ?
Le bouton **« Ouvrir l'éditeur GitHub »** reste caché, mais **Copier** et
**Télécharger** fonctionnent toujours — tu colles ou uploades le fichier toi-même.

### Deux personnes qui modifient en même temps ?
Comme pour n'importe quel fichier Git : le second à committer peut voir un conflit
si vous avez touché les deux au même moment. Pour un carnet à deux mis à jour de
temps en temps, ça n'arrive presque jamais. Si ça arrive, GitHub vous montre le
conflit sur `games.json` et vous demande de choisir/fusionner — rien de grave.

---

## Bon à savoir

- **`games.json` absent** (premier déploiement) : la page charge automatiquement
  le contenu de départ (Satisfactory en cours, Valheim / Scrap Mechanic / Albion en
  file) et t'invite tout de suite à l'enregistrer pour créer le fichier.
- **Thème clair/sombre** : la page suit le réglage de ton système.
- **La version Claude d'origine reste dispo et fonctionnelle** — celle-ci est ta
  copie, sous ton nom de domaine, sans dépendance externe.
- Tout est dans `index.html` + `games.json` : aucune dépendance à installer, aucun
  build, aucune clé API, aucun compte tiers.
