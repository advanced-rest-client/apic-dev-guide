# Code style

## Use open-wc standards

The project initially have been using own code styling. However, there's no point of continuing having own standards while there are excellent set of rules and tools defined in open-wc project.

For the sake of all involved in the project, please, use the same code style so another person working on a component can expect the same coding style.

### Open-wc setup

The simplest way of getting started with a new  component is to run

```text
npm init @open-wc
```

To manually upgrade an existing component:

Remove ARC's linting configuration files:

* .eslintignore
* .eslintrc.js
* commitlint.config.js
* gen-tsd.json
* husky.config.js
* prettier.config.js

Add dependencies

```text
npm i -D @open-wc/eslint-config eslint eslint-config-prettier prettier typescript typescript-lit-html-plugin
```

Remove the following dependencies:

* @advanced-rest-client/eslint-config
* @advanced-rest-client/prettier-config
* @commitlint/cli
* @commitlint/config-conventional
* @polymer/gen-typescript-declarations
* karma \(it's installed with open-wc/testing-karma\)

Add the following configuration to the `package.json` file:

```text
"eslintConfig": {
    "extends": [
      "@open-wc/eslint-config",
      "eslint-config-prettier"
    ],
    "overrides": [
      {
        "files": [
          "**/demo/**/*.js",
          "**/demo/**/*.html"
        ],
        "rules": {
          "no-console": "off",
          "no-unused-expressions": "off",
          "class-methods-use-this": "off",
          "import/no-extraneous-dependencies": "off"
        }
      }
    ]
  },
  "prettier": {
    "singleQuote": true,
    "arrowParens": "always"
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": [
      "eslint --fix",
      "prettier --write"
    ]
  }
```

Create the `tsconfig.json` file:

```text
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "moduleResolution": "node",
    "lib": ["es2017", "dom"],
    "allowJs": true,
    "checkJs": true,
    "noEmit": true,
    "strict": false,
    "noImplicitThis": true,
    "alwaysStrict": true,
    "esModuleInterop": true,
    "plugins": [
      {
        "name": "typescript-lit-html-plugin"
      }
    ]
  },
  "include": [
    "**/*.js",
    "node_modules/@open-wc/**/*.js"
  ],
  "exclude": [
    "node_modules/!(@open-wc)"
  ]
}
```

