# HUB Studio MGI : règles de mise à jour (lues par les tâches quotidiennes)

Le site est https://studiomgi.github.io. La page `index.html` affiche `data.json`. Les tâches ne modifient QUE `data.json`, jamais `index.html`.

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
