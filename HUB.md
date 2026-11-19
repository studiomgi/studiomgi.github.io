# HUB Studio MGI : règles de mise à jour (lues par les tâches quotidiennes)

Le site est https://studiomgi.github.io. La page `index.html` affiche `data.json`. MISE À JOUR (19/11) : les tâches n'écrivent PLUS dans ce dépôt, car elles n'y ont pas accès. Chaque tâche écrit un fichier dans SON propre dépôt :
- Partners in Vice : `hub/piv.json` dans shinkoco/piv-videos
- Tamko : `hub/tamko.json` dans shinkoco/tamko-videos
Format : {"updated": ISO+01:00, "date_label": "Jeudi 19 novembre", "alerts": ["<Nom de la chaîne> : …"], "channel": {objet de la chaîne, format ci-dessous}}. La page lit ces fichiers en direct. `data.json` ne garde que les listes `keywords` (à la main) et des valeurs de secours.

Chaque matin, après avoir programmé ses publications, chaque tâche :
1. Clone ou met à jour `studiomgi/studiomgi.github.io` (git pull juste avant d'écrire, car les deux tâches écrivent le même fichier).
2. Met à jour UNIQUEMENT sa chaîne dans `channels` : `piv` pour Partners in Vice, `tamko` pour Tamko. Garder l'autre chaîne intacte.
3. Met à jour `updated` (ISO avec +01:00) et `date_label` (ex. « Jeudi 19 novembre »).
4. Remplace dans `alerts` ses propres alertes, préfixées par le nom de la chaîne (ex. « Partners in Vice : TikTok à reconnecter »). Garder celles de l'autre chaîne. Liste vide = « Aucune action requise ».
5. Commit + push (en cas de conflit : pull --rebase puis push).

Format d'une chaîne :
- `headline` : [début, fin en gras], phrase courte et vraie, en français.
- `summary` : 1 à 2 phrases.
- `kpis` : une carte par plateforme, au format {platform: "TikTok"|"Instagram"|"YouTube", label, detail, value (nombre ou null), unit: "vues", bar: 0 à 1 (value divisée par la meilleure valeur de la chaîne)}.
- `followers` : {label, value, goal}.
- `today` : posts du jour, triés par heure : {time "HH:MM", platform, title, status : "programmé"|"publié"|"échec"}.
- `upcoming` : 1 à 3 lignes {day, text}.
N'inventer aucun chiffre : null s'il est inconnu.

Champs supplémentaires (données Zernio / Buffer, sans recherche coûteuse) :
- `trend` : {labels: ["12/11", …] (7 derniers jours), series: {"TikTok": [vues/jour…], "Instagram": […], "YouTube": […]}}. TikTok via `analytics_get_daily_metrics` de Zernio (compte de la chaîne), Instagram et YouTube via les métriques Buffer, regroupées par jour. Mettre null pour un jour inconnu, et laisser une plateforme absente si elle n'a aucune donnée.
- `engagement` : {likes, comments, shares, followers_gain} sur 7 jours (null si inconnu). followers_gain via `accounts_get_follower_stats` de Zernio.
- `best_time` : « HH:MM » via `analytics_get_best_time_to_post` de Zernio (null si non disponible).
Un seul appel par source et par jour : ne jamais paginer longuement ni rappeler la même chose.

`keywords` (racine) : listes de hashtags et de tags par chaîne, maintenues à la main. Les tâches ne doivent JAMAIS les supprimer ni les modifier.
