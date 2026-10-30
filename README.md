# Debug Méthodique

> Skill OpenCode de débogage systématique — reproduire, isoler, hypothèses vérifiées par l'expérience, fix minimal, test de non-régression.

**ID** : `debug-methodique` · **Portée** : globale (`~/.config/opencode/skills/`) · **Langue** : FR

## Ce que fait le skill

Principe directeur : **aucune hypothèse n'est validée par raisonnement seul — seulement par une observation.**

1. **Reproduire** : commande exacte, entrées, déterminisme
2. **Isoler** : fichier le plus petit qui échoue encore, diviser le code, vérifier l'environnement
3. **Énumérer** : toutes les causes possibles, classées, y compris les banales
4. **Vérifier** : prédire puis observer — une hypothèse à la fois
5. **Corriger** : fix minimal sur la cause racine ; un contournement est signalé comme tel
6. **Non-régression** : test qui échouait avant et passe après, suite complète relancée, erreur cherchée ailleurs

## Interdits

Changer plusieurs choses à la fois, corriger sans isoler, affirmer une cause sans observation, « ça marche sur ma machine » sans investiguer la différence.

## Structure

```text
debug-methodique/
└── SKILL.md              # Workflow 6 étapes, outils par cas, rapport de fin
```

## Déclenchement

- **Automatique** : bug, erreur, comportement inattendu, échec de test
- **Manuel** : `@debug-methodique`

## Rapport de fin

Cause racine + preuve observée + fix appliqué + test ajouté ; si la cause reste inconnue, le dire et proposer la prochaine étape.

## Skills liés

- `anti-dream` (origin : hypothèse ≠ preuve), `pre-ship-review` (validation avant livraison)
