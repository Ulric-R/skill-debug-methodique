---
name: Debug Méthodique
description: Méthode de débogage systématique — reproduire, isoler, énumérer les hypothèses, vérifier chaque hypothèse par l'expérience, corriger et ajouter un test de non-régression. À utiliser dès qu'un bug, une erreur, un comportement inattendu ou un échec de test est signalé.
---

# Debug Méthodique

Principe directeur (lien avec `anti-dream`) : **aucune hypothèse n'est
validée par raisonnement seul — seulement par une observation.**
« Ça doit venir de X » n'est pas une preuve.

## Workflow

### 1. Reproduire

- Obtenir la reproduction minimale : commande exacte, entrées, environnement.
- Sans reproduction fiable : chercher d'abord à la construire
  (logs, `--verbose`, test isolé). Ne pas corriger au hasard.
- Noter : fonctionne-t-elle à chaque fois ? (déterministe vs aléatoire)

### 2. Isoler

- Réduire : fichier le plus petit / cas le plus simple qui échoue encore.
- Diviser : binaire sur le code (commenter un bloc, couper une dépendance)
  pour localiser la frontière bien/mal.
- Vérifier l'environnement : version de dépendances, variable d'env,
  OS, cache. Une différence d'environnement est une cause fréquente.

### 3. Énumérer les hypothèses

- Lister **toutes** les causes possibles, classées par probabilité —
  y compris les « banales » (mauvaise variable, fichier lu n'est pas
  celui qu'on croit, version différente, cache).
- Une seule hypothèse testée à la fois.

### 4. Vérifier chaque hypothèse

- Pour chaque hypothèse : **prédire** le résultat si elle est vraie,
  puis **observer** (log, test, breakpoint, `git bisect`).
- Confirmée → corriger ; réfutée → passer à la suivante.
- Jamais : « ça a marché après mon changement » sans savoir lequel des
  changements a fonctionné ni pourquoi (tester en isolant).

### 5. Corriger

- Fix **minimal** : le plus petit changement qui traite la cause racine,
  pas le symptôme.
- Si le fix est un contournement (catch, retry, délai), le dire
  explicitement à l'utilisateur.

### 6. Non-régression

- Ajouter (ou adapter) un test qui échouait avant et passe après.
- Relancer la suite complète : le fix ne casse rien d'autre.
- Chercher la **même** erreur ailleurs dans le code (le bug est souvent
  un pattern dupliqué).

## Outils selon le cas

| Situation | Outil |
| --- | --- |
| Échec aléatoire / ordre | `git bisect`, désactiver la concurrence, seed fixe |
| Rien ne sort | logs ciblés, `--verbose`, debugger |
| Regrétion | comparer avec le dernier commit vert (`git bisect`) |
| Dépendance | vérifier lockfile, versions résolues réellement |
| Front-end | console navigateur, network tab, reproduction isolée |

## Interdits

- Changer plusieurs choses à la fois « au cas où »
- Corriger sans avoir isolé la cause (le bug reviendra)
- Affirmer une cause sans observation qui la prouve
- « Marche sur ma machine » sans investiguer la différence

## Rapport de fin

Dire : cause racine, preuve observée, fix appliqué, test ajouté,
éventuelles zones non couvertes. Si la cause reste inconnue, le dire
clairement et proposer la prochaine étape d'investigation.
