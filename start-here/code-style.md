# Code style

## Use open-wc standards

The project initially have been using own code styling. However, there's no point of continuing having own standards while there are excellent set of rules and tools defined in open-wc project.

For the sake of all involved in the project, please, use the same code style so another person working on a component can expect the same coding style.

### Open-wc setup

The simplest way of getting started with a new  component is to run

```text
npm init @open-wc
```

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
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": [
      "eslint --fix"
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

