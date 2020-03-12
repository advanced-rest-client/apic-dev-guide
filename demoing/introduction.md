# Introduction

Demoing is an important part of a component development process. It allows the developer to visually inspect the component being developed with various configuration options. It also allows to generate automated demo page for the component for the consumers.

API Components are using own demoing system based on `@advanced-rest-client/arc-demo-helper` module. It contains base classes to create an unified demo page and web components that help present the component.

Alternative to using `arc-demo-helper` is to use, recommended by `@open-wc`, Storybook. See their documentation for [demoing](https://open-wc.org/demoing/storybook-addon-markdown-docs.html).

To start with `arc-demo-helper` first install the library as a dev dependency in your project

```text
npm install -D @advanced-rest-client/arc-demo-helper
```

The following pages describes how to build a demo page using the base components.

