# API components development process

## Bootstrapping a project

We use [open-wc recommendations](https://open-wc.org/). Be sure to read their documentation first.

To bootstrap the component's project use the following command:

```text
npm init @open-wc
```

## Developing

Well, it's up to you how you going to develop your component. However, follow the following rules.

**Use the same code style**. See [Code style ](code-style.md)documentation for more information. It makes it easier for everyone to use the same code styling. Especially it is important for linters. Commit may be accepted in your development environment but rejected in another if linter configuration is different. That makes collaboration very hard to do.

**Check if similar component already exists**. That should go without saying but we do not need duplicates. Take a look into **Versioning** section for exceptions.

**Note events design patterns**. As described in [event design](https://w3ctag.github.io/design-principles/#event-design) recommendation document, when creating an event inside the component also create `oneventname` setter that registers the event. See referenced document for more information.

**Always add documentation, demo, and tests**. No exceptions here. Each document must have demo page. See corresponding sections below for more information.

## Accessibility

It is your responsibility to add support for a11y in your component. This means, if applicable, add `role` and `aria-*` attributes to your components. If your components is interactive make sure it is focusable. If your component support states like `checked` or `disabled` use an appropriate aria attributes. Finally tests the components for accessibility in your test cases \(described below\).

Read this [Accessibility Principles](https://www.w3.org/WAI/fundamentals/accessibility-principles/) to get started with accessible web.

## Demoing

The demo page is a part of the development process. It is required for all API Components. There are 2 main reasons to do this.

First reason is the you as the author can design a better component. You can visually inspect the component and debug it. Try to create different demo states like with different options enabled or with different composition of the components. If the element has a label or text input then create a demo case that has `dir="ltr"` attribute.

Ideally would be having a demo page that applies different styles via CSS variables and CSS parts. Try to create a dark theme of the component. This is helpful to predict which styling API to expose before publishing the element.

Use open-wc recommendations for [demoing](https://open-wc.org/demoing/) if you don't have your own system.

## Documenting

You won't be the only person developing the component. There will be others that will introduce new functionality or fix a bug. Make their life easier and document the code. Do it also for yourself. In few weeks you won't be remember what the code does.

Public functions \(not staring with `_`\) must be documented. This is your component's public API and authors must know how to use it.

```text
/**
 * Selects an item for given index.
 * @param {Number} index An index of an item to be selected
 * @return {HTMLElement} Reference to newly selected element
 * @throws {TypeError} When passed argument is not a number.
 * @throws {RangeError} When index is lower than zero or higher than number of items.
 */
select(index) {
  if (isNaN(index)) {
    throw new TypeError(`Expected number, ${typeof index} given.`);
  }
  if (index > this.items.length - 1 || index < 0) {
    throw new RangeError('Index out of bounds.');
  }
  this.selected = index;
  this._updateSelection();
  return this.selectedItem;
}
```

Please, add `keywords` to `package.json` file. This keywords are used by the components catalogue.

```text
{
  "keywords": [
    "web-components",
    "button",
    "radio-button",
    "radio",
    "form-elements"
  ]
}
```

Currently it is mandatory to call `npm run update-types` before publishing the component manually. The CI pipeline will handle this in the future. This command generates typings for the component so it can be used in TS projects.

## Testing

Follow rules from `@open-wc` for testing. Projects created with `@open-wc` uses Karma to run tests.

### a11y

To test for accessibility use [a11y-axe](https://open-wc.org/testing/testing-chai-a11y-axe.html). This will detect some accessibility issues, if any. Try to test for different states, like focused, disabled, active, etc. Don't forget to do manual testing if your `aria` or `role` attributes are set correctly.

```text
import { fixture, assert } from '@open-wc/testing';

describe('a11y', () => {
  async function basicFixture() {
    return (await fixture(`<anypoint-radio-button>Label</anypoint-radio-button>`));
  }

  async function roleFixture() {
    return (await fixture(`<anypoint-radio-button role="checkbox"></anypoint-radio-button>`));
  }

  async function tabindexFixture() {
    return (await fixture(`<anypoint-radio-button tabindex="-1"></anypoint-radio-button>`));
  }

  it('is accessible in normal state', async () => {
    const element = await fixture(`<anypoint-radio-button>Label</anypoint-radio-button>`);
    await assert.isAccessible(element);
  });

  it('is accessible in disabled state', async () => {
    const element = await fixture(`<anypoint-radio-button disabled>Label</anypoint-radio-button>`);
    await assert.isAccessible(element);
  });

  it('is accessible in checked state', async () => {
    const element = await fixture(`<anypoint-radio-button checked>Label</anypoint-radio-button>`);
    await assert.isAccessible(element);
  });

  it('is not accessible without the label', async () => {
    const element = await fixture(`<anypoint-radio-button></anypoint-radio-button>`);
    await assert.isNotAccessible(element);
  });

  it('is accessible with aria-label', async () => {
    const element = await fixture(`<anypoint-radio-button aria-label="Batman"></anypoint-radio-button>`);
    await assert.isNotAccessible(element);
  });

  it('Sets default role attribute', async () => {
    const element = await basicFixture();
    assert.equal(element.getAttribute('role'), 'radio');
  });

  it('Respects existing role attribute', async () => {
    const element = await roleFixture();
    assert.equal(element.getAttribute('role'), 'checkbox');
  });

  it('Sets default tabindex attribute', async () => {
    const element = await basicFixture();
    assert.equal(element.getAttribute('tabindex'), '0');
  });

  it('Respects existing tabindex attribute', async () => {
    const element = await tabindexFixture();
    assert.equal(element.getAttribute('tabindex'), '-1');
  });
});
```

## Committing

`@open-wc` generated project provides rules for commit messages using Husky.

See the commit rules in [config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional) package used by `@open-wc`.

## Components versioning

It is very important section so please, don't ignore it.

API Components are build in a way that should not require major releases in the future. If you read and follow rules of [API components architecture overview](api-components-architecture-overview.md) you are almost onboard with this philosophy. So far there were 2 major updates to the components but not because the requirement for the component changed but rather underlying technology \(HTML spec\).

The general rules is if you need to create a breaking change in existing component then most probably you actually need a new component. The old component probably works well in different application or environment and should not be changed so drastically. If your context has changed then build a new component that might be similar but serves actually different purpose.

This way we won't create a mess with versioning and upgrading and applications consuming the components can use pattern like `^1.0.0` when installing the component as it will never introduce breaking change.

When deprecating an API in your component just leave the old API there but without the actual implementation but with the replacement. We don't want breaking changes as much as we don't like upgrading legacy application for new API. This application should still be working even when upgrading version of a component.

