# Présentation de soutenance — Projet CYBEL

Support de préparation pour une soutenance orale de 10 minutes.
11 slides, durée totale estimée : **575 secondes (≈ 9 min 35)**.

---

# Slide 1

## Objectif du slide
Accrocher le jury et poser en une phrase le sujet du stage.

## Titre
CYBEL — Piloter un robot fermé, sans son application

## Contenu
- Projet CYBEL — plateforme de commande robot
- Robot CIOT TY1251D-03195 (HESTIM)
- Stage ingénieur — Génie Informatique & IA
- Claudia Lusamote Kimfuta, 2025-2026

## Visuel recommandé
**Photo** — photo pleine largeur du robot CIOT (cadrage large, robot entier visible), logo HESTIM en petit dans un coin, titre du projet en surimpression en bas de l'image.

## Notes du présentateur
« Bonjour, je m'appelle Claudia Lusamote Kimfuta, et je vais vous présenter mon projet de stage : CYBEL. HESTIM dispose d'un robot de réception, le CIOT TY1251D-03195, mais ce robot est livré avec une application propriétaire totalement fermée — impossible de la personnaliser ou de l'adapter aux besoins du laboratoire. L'objectif de mon stage a été de construire, à partir de zéro, une plateforme indépendante pour piloter ce robot sans jamais dépendre de cette application d'origine. »

## Temps estimé
30 secondes

---

# Slide 2

## Objectif du slide
Faire comprendre concrètement le verrou technique qui justifie tout le projet.

## Titre
Un robot... sans mode d'emploi

## Contenu
- Châssis ROS + tête Android
- Application propriétaire fermée
- Aucun protocole documenté
- Aucune API publique

## Visuel recommandé
**Schéma** — robot au centre, une bulle "application propriétaire" avec une icône de cadenas dessus, des flèches en pointillés bloquées vers l'extérieur pour symboliser l'absence d'accès.

## Notes du présentateur
« Le robot est en réalité composé de deux parties : un châssis qui roule sous ROS, et une tête Android qui gère l'accueil des visiteurs. Le problème, c'est que tout passe par une application constructeur boîte noire : pas de documentation, pas de code source, pas d'API. Concrètement, HESTIM ne pouvait ni personnaliser les scénarios d'accueil, ni superviser le robot à distance, ni l'intégrer à d'autres outils. Il fallait donc comprendre ce système de l'intérieur, sans aucune aide du constructeur. »

## Temps estimé
55 secondes

---

# Slide 3

## Objectif du slide
Énoncer clairement la question de recherche et les objectifs qui en découlent.

## Titre
La problématique

## Contenu
- Piloter le robot sans l'application
- Reconstruire le protocole par observation
- Bâtir une plateforme robuste et extensible
- Sans décompiler la logique métier

## Visuel recommandé
**Schéma / citation** — la question centrale affichée en grand, dans un encadré sobre au centre de la diapositive, sans autre texte autour.

## Notes du présentateur
« La question centrale du stage était la suivante : comment concevoir une plateforme de commande tierce pour ce robot, alors qu'aucune documentation officielle n'existe sur ses interfaces internes ? Cette problématique a deux volets : d'abord établir une communication fiable avec un système dont on ne connaît ni le protocole ni les points d'accès, puis construire à partir de cette compréhension une solution robuste et réutilisable. Une contrainte forte a guidé tout le travail : pas question de décompiler la logique métier de l'application constructeur, uniquement observer le trafic réseau et les interfaces standard. »

## Temps estimé
55 secondes

---

# Slide 4

## Objectif du slide
Mettre en valeur la rigueur méthodologique, l'élément le plus différenciant du travail.

## Titre
Une démarche prudente, étape par étape

## Contenu
- Observation passive du trafic
- Test isolé en script
- Vérification de l'effet réel
- Intégration au SDK
- Documentation continue

## Visuel recommandé
**Chronologie / schéma circulaire** — cycle en 5 étapes avec flèches reliant chaque étape à la suivante (réutiliser le diagramme `processColor.png` du rapport, simplifié si besoin).

## Notes du présentateur
« Face à un système fermé et à un robot unique, disponible seulement quelques jours par semaine, chaque hypothèse sur le protocole a suivi un cycle strict : observation passive d'abord, sans rien publier ; puis un test isolé dans un script dédié ; ensuite une vérification que la commande est réellement exécutée sur le robot — et pas seulement acceptée par le pont de communication, ce qui n'est pas la même chose ; puis intégration dans le SDK, et documentation systématique. Cette rigueur nous a évité de casser quoi que ce soit sur un matériel qu'on ne pouvait pas se permettre d'endommager. »

## Temps estimé
60 secondes

---

# Slide 5

## Objectif du slide
Présenter la solution technique construite, dans une architecture claire et compréhensible.

## Titre
Architecture de la plateforme CYBEL

## Contenu
- SDK Python : mode simulé + réel
- API FastAPI (REST + WebSocket)
- Interface opérateur et visiteur
- Rosbridge / MQTT vers le robot

## Visuel recommandé
**Architecture** — diagramme en couches (réutiliser `architecture_couches.png` ou `architecture_generale.png`), du robot en bas jusqu'aux interfaces en haut, avec flèches de communication.

## Notes du présentateur
« La plateforme est organisée en couches. Tout en bas, le robot, accessible via rosbridge et MQTT. Au-dessus, un SDK Python qui propose deux implémentations interchangeables : une version simulée et une version réelle — ce qui nous a permis de développer sans dépendre en permanence du robot physique. Ensuite, une API FastAPI qui orchestre tout, et enfin les interfaces web : une pour l'opérateur, une pour le visiteur. Cette séparation stricte facilite les tests et l'évolution du système. »

## Temps estimé
60 secondes

---

# Slide 6

## Objectif du slide
Démontrer le résultat concret et le plus technique de la rétro-ingénierie.

## Titre
Le protocole, reconstruit pièce par pièce

## Contenu
- Topologie réseau à 3 segments
- Topics ROS via rosbridge (WebSocket)
- Statuts de navigation (600 à 604)
- Commande acceptée ≠ commande exécutée

## Visuel recommandé
**Capture d'écran + tableau court** — capture du scan Zenmap (`port.png`) à gauche, petit tableau à droite avec 3-4 topics clés et leur usage (ex. `/robot_pose`, `/navi_goal`, `/mobile_base/commands/velocity`).

## Notes du présentateur
« Concrètement, la rétro-ingénierie a permis de cartographier trois segments réseau distincts, d'identifier les services exposés — rosbridge sur le port 9090, MQTT sur le 1883 — et de reconstruire les topics utiles à la navigation et à la télémétrie. Le point le plus délicat a été de comprendre que rosbridge peut accepter une commande sans qu'elle soit réellement exécutée par le châssis : il a fallu développer une méthode de vérification systématique, via l'introspection ROS, pour s'assurer qu'une commande envoyée avait un effet réel sur le robot. »

## Temps estimé
60 secondes

---

# Slide 7

## Objectif du slide
Montrer concrètement ce qui a été livré, côté utilisateur final.

## Titre
Deux interfaces, deux usages

## Contenu
- Opérateur : carte, LiDAR, téléopération
- Visiteur : kiosque bilingue FR/EN
- Visite guidée autonome, 8 arrêts
- Déployé directement sur la tablette

## Visuel recommandé
**Capture d'écran** — deux captures côte à côte : le tableau de bord opérateur (`dashbordcontroleur.png`) à gauche, l'écran du kiosque visiteur (`appvisit.jpeg`) à droite.

## Notes du présentateur
« Deux interfaces ont été développées. Côté opérateur : une carte SLAM en temps réel, le nuage de points LiDAR, la téléopération manuelle et la gestion de la visite guidée. Côté visiteur : un kiosque tactile bilingue français-anglais, installé directement sur la tablette du robot, qui propose une visite guidée autonome du laboratoire en huit arrêts, sans intervention humaine. Cette visite complète a été validée sans erreur sur le terrain. »

## Temps estimé
60 secondes

---

# Slide 8

## Objectif du slide
Expliquer le canal vocal, un défi technique à part, et poser clairement une limite du périmètre.

## Titre
Faire parler le robot

## Contenu
- TTS absent du protocole ROS
- Application Android dédiée
- Déclenchement par ADB / broadcast
- Reconnaissance vocale : hors périmètre CYBEL

## Visuel recommandé
**Schéma de flux** — chaîne simple : Backend CYBEL → ADB / broadcast → application `CybelTTSBridge` → haut-parleur du robot.

## Notes du présentateur
« Un défi inattendu : la synthèse vocale ne transite pas du tout par ROS. Il a fallu développer une application Android dédiée, CybelTTSBridge, déclenchée depuis le backend via ADB ou par un broadcast local sur la tablette. Pour être transparente : la reconnaissance vocale et l'interaction conversationnelle avec le robot ne font pas partie de mon périmètre — elles sont traitées par un module chatbot développé en parallèle par mon collègue de stage, sur une branche séparée du projet. »

## Temps estimé
45 secondes

---

# Slide 9

## Objectif du slide
Convaincre avec des résultats chiffrés que le système fonctionne réellement.

## Titre
Ça fonctionne — les résultats

## Contenu
- 78 tests unitaires validés
- 10 scénarios terrain validés
- Visite guidée complète réussie
- Fiabilité connexion rosbridge ≈ 90 %

## Visuel recommandé
**Tableau court + pictogrammes** — 4 lignes avec une icône de validation (✓) à côté de chaque résultat ; éviter tout tableau dense, garder uniquement les chiffres clés.

## Notes du présentateur
« Sur le plan des résultats : 78 tests unitaires automatisés passent tous au vert, et les dix scénarios de test terrain — téléopération, navigation, TTS, kiosque — sont validés, y compris la visite guidée complète des huit arrêts, réalisée sans erreur. Le système reste dépendant de la qualité du Wi-Fi, avec une fiabilité de connexion autour de 90 % en conditions réelles, ce qui reste le principal facteur de variabilité observé. »

## Temps estimé
60 secondes

---

# Slide 10

## Objectif du slide
Montrer l'esprit critique et l'honnêteté scientifique, anticiper les questions du jury.

## Titre
Ce qu'il reste à faire

## Contenu
- Dépendance totale au Wi-Fi du robot
- Un seul robot testé
- Pas de CI/CD
- Sécurité réseau à renforcer

## Visuel recommandé
**Liste à icônes** — 4 icônes simples (Wi-Fi, robot unique, engrenage, cadenas), sans graphique complexe ; alternative : tableau à 2 colonnes priorité/amélioration si le temps le permet.

## Notes du présentateur
« Comme tout projet de stage, CYBEL a des limites que j'assume clairement. Le système dépend entièrement du Wi-Fi du robot : pas de contrôle possible en dehors de ce réseau. Un seul exemplaire de robot a été testé, donc la généralisation à d'autres modèles n'est pas garantie. Il n'y a pas encore de CI/CD ni de déploiement industrialisé. Et les canaux rosbridge et MQTT étant accessibles sans authentification, un renforcement de la sécurité réseau serait nécessaire pour une exploitation à long terme. »

## Temps estimé
50 secondes

---

# Slide 11

## Objectif du slide
Clore sur un message fort et mémorable, remercier le jury.

## Titre
Un robot fermé, une plateforme ouverte

## Contenu
- Objectif atteint : piloter sans l'application
- Protocole documenté et réutilisable
- Compétences réseau, ROS, full-stack
- Merci — questions ?

## Visuel recommandé
**Photo** — reprise de la photo du robot utilisée en slide 1 (effet de boucle visuelle), avec le logo HESTIM et la mention "Merci".

## Notes du présentateur
« Pour conclure : ce stage démontre qu'un robot de service fermé peut être intégré dans un écosystème personnalisé, à condition d'accepter une phase d'investigation rigoureuse et une discipline de validation stricte. Le protocole reconstruit et la documentation produite permettent à d'autres étudiants de reprendre ce travail sans repartir de zéro. Je vous remercie pour votre attention, et je suis à votre disposition pour vos questions. »

## Temps estimé
40 secondes

---

# Analyse globale

- **Durée totale** : 30+55+55+60+60+60+60+45+60+50+40 = **575 secondes ≈ 9 min 35** — tient dans les 10 minutes avec ~25s de marge pour les transitions entre slides.
- **Progression logique** : contexte → problème → objectifs → méthode → architecture → protocole → interfaces → interaction vocale → résultats → limites → conclusion. C'est une progression classique et efficace : chaque slide répond à la question que la précédente a fait naître dans l'esprit du jury.
- **Aucune répétition détectée** : contrairement au rapport écrit (où la topologie réseau est décrite 3 fois), ici chaque information n'apparaît qu'une seule fois, au bon endroit.
- **Chaque slide apporte une information nouvelle** : aucune slide ne reformule une précédente ; slide 4 (méthode) et slide 6 (protocole) pourraient sembler proches mais l'une parle du *comment on a cherché*, l'autre du *ce qu'on a trouvé* — la distinction doit être verbalisée clairement à l'oral pour éviter toute impression de redite.
- **Messages clés bien mis en avant** : le message central (« piloter un robot fermé sans son application, en le prouvant par une démarche rigoureuse ») ressort dès la slide 1 et est repris en clôture — bonne boucle narrative.
- **Contenu volontairement écarté du rapport** (à ne pas mettre en slide, à mentionner à l'oral si le jury demande) : présentation détaillée de HESTIM (chapitre 1), tableaux complets des exigences EF/ENF, benchmark comparatif détaillé de l'état de l'art, bibliographie, répartition fine des contributions entre stagiaires — tout cela reste disponible dans le rapport mais alourdirait inutilement l'oral.

---

# Conseils de design

| Slide | Animation utile ? | Éléments progressifs | Couleur à mettre en avant | Éléments fixes |
|---|---|---|---|---|
| 1 | Non — image + titre fixes dès l'ouverture | — | Couleurs du logo HESTIM (bleu/orange) en accent | Photo robot, titre |
| 2 | Oui — le cadenas apparaît après le robot | Icône cadenas en second | Rouge/gris pour signaler le blocage | Robot au centre |
| 3 | Oui — la question apparaît mot à mot ou en un seul bloc après un silence de 2s | La citation seule, en dernier | Une seule couleur d'accent, sobre | Cadre de la citation |
| 4 | Oui — chaque étape du cycle apparaît une à une en tournant | Chaque étape à son tour | Une couleur par étape (5 teintes proches, pas un arc-en-ciel) | Le cercle/structure du cycle |
| 5 | Oui — les couches apparaissent du bas (robot) vers le haut (interfaces) | Chaque couche | Une couleur par couche, cohérente avec la légende du rapport | Les flèches de connexion |
| 6 | Non — trop technique pour de l'animation, tout doit être visible d'un coup | — | Mettre en avant les 2-3 topics les plus parlants en gras/couleur | Capture Zenmap |
| 7 | Oui — faire apparaître les deux captures l'une après l'autre en les commentant | Capture opérateur puis capture kiosque | Couleur neutre, laisser parler les captures | Titre de la slide |
| 8 | Oui — la chaîne de flux se construit étape par étape (Backend → ADB → App → Haut-parleur) | Chaque flèche | Couleur d'alerte discrète sur "hors périmètre" | Icône haut-parleur |
| 9 | Oui — chaque chiffre apparaît avec un léger effet de comptage ou fondu | Les 4 résultats un par un | Vert pour les validations (✓) | Titre |
| 10 | Non, ou très sobre — éviter de dramatiser des limites avec de l'animation | — | Gris/orange neutre, pas de rouge alarmiste | Les 4 icônes |
| 11 | Non — clôture sobre | — | Reprendre la couleur d'accent de la slide 1 (boucle visuelle) | Photo, logo |

---

# Conseils de soutenance

**Slide 1 — Titre**
- Piège : passer trop de temps ici par nervosité. Rester à 30s strict.
- Question probable : « Pourquoi ce robot en particulier / pourquoi HESTIM ? » — réponse courte prête (mise à disposition dans le cadre du stage, FabLab).

**Slide 2 — Contexte**
- Piège fréquent : sur-expliquer les caractéristiques techniques du robot (RK3399, Android 7.1) qui n'intéressent pas le jury ici — garder ça pour les questions.
- Question probable : « Le constructeur ne fournit vraiment aucune documentation, même sous NDA ? » — dis clairement que non, aucune n'a été obtenue.

**Slide 3 — Problématique**
- Piège : utiliser le mot « supérieure » (à l'application propriétaire) sans pouvoir le justifier immédiatement si on te challenge — préfère dire oralement « au moins équivalente, sur le périmètre visé ».
- Question probable : « Pourquoi ne pas avoir décompilé l'application, ça aurait été plus rapide ? » — réponse sur la contrainte de propriété intellectuelle et l'éthique du stage.

**Slide 4 — Méthodologie**
- Erreur fréquente : décrire la méthode de façon trop abstraite. Toujours illustrer avec un exemple concret (ex. mode manuel/auto qui bloque la téléopération).
- Question probable : « Comment vérifiez-vous qu'une commande a été réellement exécutée ? » — tu as la réponse toute prête (`/rosapi/subscribers`), utilise-la, c'est ton meilleur argument technique.

**Slide 5 — Architecture**
- Piège : rentrer dans le détail de chaque endpoint API — reste au niveau des couches, garde le détail pour les questions.
- Question probable : « Pourquoi FastAPI et pas un autre framework ? », « Le mode mock, comment garantit-il la fidélité au comportement réel ? »

**Slide 6 — Protocole**
- Piège : lire le tableau au lieu de le commenter — ne cite pas tous les topics, choisis-en 2-3 parlants.
- Question probable : « Ces canaux ne sont pas authentifiés — n'est-ce pas un risque de sécurité ? » — assume-le complètement, c'est une vraie limite que tu as identifiée, ne cherche pas à minimiser.

**Slide 7 — Interfaces**
- Erreur fréquente : montrer les captures sans les commenter en marchant dessus verbalement — nomme chaque élément visible à l'écran pendant que tu parles.
- Question probable : « La visite guidée est-elle totalement autonome, sans supervision humaine possible ? » — clarifie qu'un E-Stop opérateur reste disponible.

**Slide 8 — Interaction vocale**
- Piège important : ne pas se laisser piéger sur le périmètre — si le jury demande « et la reconnaissance vocale ? », rester ferme et clair que c'est un module séparé, développé par un autre stagiaire, pas dans ton périmètre.
- Question probable : « Pourquoi ADB et pas une solution réseau plus simple ? » — expliquer l'absence de tout canal ROS/réseau pour la parole sur cette tête Android.

**Slide 9 — Résultats**
- Piège classique : donner un chiffre (90 %) sans pouvoir dire comment il a été mesuré si on te le demande — prépare une phrase courte sur les conditions de mesure (sessions terrain, dépendance Wi-Fi).
- Question probable : « 90 %, c'est suffisant pour un usage réel avec du public ? » — reconnaître que non, pas encore, d'où les perspectives d'amélioration réseau.

**Slide 10 — Limites**
- Erreur fréquente : sembler sur la défensive en présentant les limites — au contraire, présente-les avec assurance, ça inspire confiance en ton sens critique.
- Question probable : « Laquelle de ces limites est la plus urgente à traiter ? » — avoir une réponse hiérarchisée prête (sécurité réseau ou calibrage, selon ta priorité réelle).

**Slide 11 — Conclusion**
- Piège : finir sur une note trop vague ("j'ai beaucoup appris") — reste concret sur la contribution technique et la réutilisabilité pour HESTIM.
- Question probable : « Que referiez-vous différemment avec plus de temps ? » — prépare une réponse honnête et courte (ex. tests d'intégration réseau simulés dès le départ).
