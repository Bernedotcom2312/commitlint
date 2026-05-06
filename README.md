# Commitlint + Husky PoC

Mise en place des [Conventional Commits](https://www.conventionalcommits.org/) via **commitlint** et **husky**.

> Pour en savoir plus, consultez mon article : <https://valerian-pyckaert.dev/posts/commitlint>

---

## Prérequis

- [Node.js](https://nodejs.org/) >= 25
- [Git](https://git-scm.com/)

## Installation

```bash
npm install
```

La commande `npm install` exécute automatiquement le script **prepare** qui initialise les hooks Git via Husky.

## Stack

| Outil | Rôle |
|---|---|
| [commitlint](https://commitlint.js.org/) | Valide le format des messages de commit |
| [@commitlint/config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional) | Règles basées sur la spec Conventional Commits |
| [husky](https://typicode.github.io/husky/) | Gère les Git hooks (ici `commit-msg`) |

## Convention de commit

Chaque message de commit doit respecter le format suivant :

```
<type>(<scope>): <description>

[body]

[footer(s)]
```

### Types autorisés

| Type | Description |
|---|---|
| `feat` | Ajout d'une nouvelle fonctionnalité |
| `fix` | Correction d'un bug |
| `docs` | Modification de la documentation |
| `style` | Changement de style (formatage, espaces, etc.) |
| `refactor` | Refactorisation du code (ni fix, ni feat) |
| `perf` | Amélioration des performances |
| `test` | Ajout ou modification de tests |
| `build` | Changement du système de build ou des dépendances |
| `ci` | Modification de la configuration CI |
| `chore` | Tâches diverses (maintenance, outillage) |
| `revert` | Annulation d'un commit précédent |

### Exemples

```bash
# Valide
git commit -m "feat(auth): add JWT token refresh"
git commit -m "fix: resolve null pointer on login"
git commit -m "docs: update README with commit convention"

# Invalide — sera rejeté par commitlint
git commit -m "updated stuff"
git commit -m "WIP"
```

### Breaking Changes

Pour signaler un changement non rétrocompatible, ajoutez `!` après le type/scope ou un footer `BREAKING CHANGE:` :

```bash
git commit -m "feat(api)!: remove deprecated /v1 endpoints"
```

## Structure du projet

```
.
├── .husky/
│   └── commit-msg          # Hook Git — exécute commitlint sur le message
├── commitlint.config.js    # Configuration commitlint
├── package.json
└── README.md
```

## Configuration

La configuration de commitlint se trouve dans [commitlint.config.js](commitlint.config.js) :

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
};
```

Vous pouvez personnaliser les règles en ajoutant une clé `rules`. Exemple :

```js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'header-max-length': [2, 'always', 120],
    'scope-enum': [2, 'always', ['api', 'ui', 'core', 'docs']],
  },
};
```

Consultez la [documentation des règles](https://commitlint.js.org/reference/rules.html) pour toutes les options disponibles.

## Tester localement

```bash
# Valider un message manuellement
echo "feat: add login page" | npx commitlint

# Tester un message invalide
echo "bad commit" | npx commitlint
# => ✖ subject may not be empty
# => ✖ type may not be empty
```
