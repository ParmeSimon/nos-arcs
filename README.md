# Nos Arcs — carnet de campagne coop

Une page web pour suivre les jeux que vous enchaînez à deux : arc en cours, file
d'attente, jeux terminés et recommandations basées sur vos genres.

**Où ça vit :** tu n'as pas d'hébergement payant pour `simonparme.com`, donc tout
est servi **gratuitement par GitHub Pages**. L'app est dans un sous-dossier
`arc/`, ce qui donne directement l'URL `simonparme.com/arc` — aucune config DNS
particulière au-delà de celle déjà nécessaire pour brancher ton domaine.

**Comment ça marche :** les données vivent dans `arc/games.json`. La page les lit
au chargement. Tes changements dans l'interface (ajouter un jeu, le terminer, le
noter…) apparaissent tout de suite à l'écran ; pour qu'ils soient définitifs et
visibles par ton pote, tu les « Enregistres » (bouton dans la page) en remplaçant
le contenu de `arc/games.json` sur GitHub.

---

## Structure du dépôt

```
CNAME              → domaine personnalisé pour GitHub Pages (simonparme.com)
index.html         → petite page qui redirige simonparme.com vers simonparme.com/arc
arc/
  index.html        → toute l'app (design + logique)
  games.json         → vos données (le seul fichier que vous modifiez au fil du temps)
```

## Étape 1 — Mettre le code sur GitHub

Depuis le dossier qui contient `CNAME`, `index.html` et `arc/` :

```bash
git init
git add .
git commit -m "Nos Arcs — carnet coop"
git branch -M main
# Crée d'abord un dépôt vide sur github.com (ex. "nos-arcs"), puis :
git remote add origin https://github.com/parmesimon/nos-arcs.git
git push -u origin main
```

Remplace `nos-arcs` par le nom que tu donnes à ton dépôt. Il doit être **public**
pour GitHub Pages gratuit.

> C'est cette étape (et la suivante) qui nécessite **ton** compte : je ne peux pas
> pousser à ta place, ni toucher à tes identifiants.

## Étape 2 — Activer GitHub Pages

1. Sur GitHub : **Settings → Pages**.
2. *Source* : **Deploy from a branch**.
3. *Branch* : **main**, dossier **/ (root)** → **Save**.
4. Au bout d'une minute, vérifie que ça marche sur
   `https://parmesimon.github.io/nos-arcs/arc/` (adapte le nom du dépôt) —
   **avant** de brancher le domaine.

## Étape 3 — Brancher simonparme.com

**A. Côté GitHub** — *Settings → Pages → Custom domain* : tape `simonparme.com`, **Save**.
Coche **Enforce HTTPS** une fois le certificat prêt (peut prendre quelques minutes).
C'est exactement l'écran que tu m'as montré en capture.

**B. Côté DNS**, chez OVH (là où est ton nom de domaine) : va dans
**Noms de domaine → simonparme.com → Zone DNS**, et ajoute quatre enregistrements
**A** sur l'apex `@` :

```
Type  Sous-domaine  Cible
A     (vide / @)     185.199.108.153
A     (vide / @)     185.199.109.153
A     (vide / @)     185.199.110.153
A     (vide / @)     185.199.111.153
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
Type   Sous-domaine   Cible
CNAME  www            parmesimon.github.io.
```

⚠️ Si OVH te propose déjà des enregistrements par défaut sur `@` (souvent une
redirection OVH ou un enregistrement A vers un serveur OVH), **supprime-les**
avant d'ajouter les quatre A ci-dessus — sinon ça reste sur l'ancienne cible.

Compte de quelques minutes à ~1 h de propagation DNS. Une fois propagé, va sur
**Settings → Pages** dans GitHub : le message doit passer au vert (domaine
vérifié), puis coche **Enforce HTTPS**.

## Étape 4 — Relier le bouton « Enregistrer » à ton dépôt

Ouvre `arc/index.html`, tout en bas, et renseigne ton dépôt exact :

```html
<script>
window.REPO = { owner: "parmesimon", name: "nos-arcs", branch: "main" };
window.DATA_FILE = "games.json";
</script>
```

Avec ça, le bouton **« Ouvrir l'éditeur GitHub »** de la fenêtre d'enregistrement
t'amène directement sur la page d'édition de `arc/games.json`, sur la bonne branche.

---

## Utilisation au quotidien

1. Sur `simonparme.com/arc`, ajoute un jeu, marque-le terminé, note-le, réordonne
   la file… tout se met à jour à l'écran immédiatement.
2. Une **barre en bas** apparaît dès qu'il y a des changements non enregistrés :
   clique **« Enregistrer… »**.
3. Une fenêtre s'ouvre avec le contenu à jour de `games.json` (et, en option, un
   onglet `arcs.md` — une version lisible, si tu veux une jolie page markdown sur
   GitHub en plus).
4. Clique **Copier**, puis **Ouvrir l'éditeur GitHub** — ça t'amène direct sur la
   page d'édition de `arc/games.json`. Sélectionne tout (Ctrl/Cmd+A), colle, puis
   en bas de la page GitHub clique **Commit changes**.
5. Reviens sur la fenêtre et clique **« C'est commité »**.
6. GitHub Pages redéploie en général en **moins d'une minute**. Ton pote (et toi,
   après un rechargement) verra la mise à jour.

Pas envie d'ouvrir GitHub tout de suite ? Le bouton **Télécharger** te donne le
fichier en local, à uploader plus tard.

### Deux personnes qui modifient en même temps ?
Comme pour n'importe quel fichier Git : le second à committer peut voir un conflit
si vous avez touché les deux au même moment. Pour un carnet à deux mis à jour de
temps en temps, ça n'arrive presque jamais. Si ça arrive, GitHub vous montre le
conflit sur `games.json` et vous demande de choisir/fusionner — rien de grave.

---

## Bon à savoir

- **`arc/games.json` absent** (premier déploiement) : la page charge automatiquement
  le contenu de départ (Satisfactory en cours, Valheim / Scrap Mechanic / Albion en
  file) et t'invite tout de suite à l'enregistrer pour créer le fichier.
- **La racine `simonparme.com`** redirige automatiquement vers `/arc/` — pratique
  tant que tu n'as rien d'autre sur ce domaine. Le jour où tu veux un vrai site
  d'accueil, remplace simplement le contenu de `index.html` (à la racine) par ce
  site, et laisse `arc/` tel quel.
- **Thème clair/sombre** : la page suit le réglage de ton système.
- Aucune dépendance à installer, aucun build, aucune clé API, aucun compte tiers —
  juste GitHub Pages (gratuit) et ton nom de domaine chez OVH.
