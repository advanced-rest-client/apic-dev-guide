# Building a demo page

The `arc-demo-helper` module contains base classes and custom elements used in most of API Components demo pages. To keep consistency it is encouraged to use this classes to build unified experience when working with the demo pages.

Demo pages uses `lit-html` library as a templating system. This is the same library used by LitElement to create template for a component.

To build a demo page for a component use `DemoPage` class as a base class of the demo page. It has a basic set of methods that help build unified demo page.

## Basic Example

`index.html`

```markup
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, minimum-scale=1, initial-scale=1, user-scalable=yes">
    <title>Demo page</title>
  </head>
  <body class="styled">
    <div id="demo"></div>

    <script type="module" src="./index.js"></script>
  </body>
</html>
```

`index.js`

```javascript
import { html } from 'lit-html';
import { DemoPage } from '@advanced-rest-client/arc-demo-helper';

class ComponentDemo extends DemoPage {
  contentTemplate() {
    return html`<p>demo content</p>`;
  }
}

const instance = new ComponentDemo();
instance.render();
```

The demo page should extend `DemoPage` class that has all base methods and configuration options. The implementation must override `contentTemplate()` function that must return a template to render in the content area.

Rendering is handled by the `DemoPage` class. The initial rendering must be called manually after creating an instance of the demo page. Use `render()` function to manually trigger the render function. The render is called asynchronously.

## Observable properties

The `DemoPage` has `initObservableProperties()` method that registers properties set on the demo page as "observable". When a value of the property change then the view is re-rendered. The operation is deferred so it is safe to set multiple properties at the same time.

```javascript
class ComponentDemo extends DemoPage {
  constructor() {
    super();
    this.initObservableProperties([
      'prop1', 'prop2'
    ]);
  }

  someMethod() {
    this.prop1 = 'value 1';
    this.prop2 = 'value 2';
  }

  contentTemplate() {
    const { prop1, prop2 } = this;
    return html`<p>
      prop1: ${prop1}, prop2: ${prop2}
    </p>`;
  }
}
```

Deep properties changes are not observed. If a deep property change then call `render()` function on the demo page.

## Component title

Define a constructor with `componentName` property set to the component name. This renders the component name in the title bar. The title supports a11y.

```javascript
class ComponentDemo extends DemoPage {
  constructor() {
    super();
    this.componentName = 'my-component';
  }

  ...
}
```

## Basic view options

By setting `renderViewControls` property you enable some basic UI controls to change the behavior of the demo page. Basic options are:

* Toggling **dark theme**
* Toggling **narrow** property in the template
* Toggling **styles** applied to the component



```javascript
class ComponentDemo extends DemoPage {
  constructor() {
    super();
    this.componentName = 'my-component';
    this.renderViewControls = true;
  }

  ...
}
```

This renders a settings drop down menu that allows to toggle some basic options.

When the "**narrow**" option is toggled it toggled a boolean value of the `narrow` property of the demo page. Some component uses this value to render a mobile friendly view of the component. If your component has different view for mobile and desktop then this property should be supported.

```javascript
contentTemplate() {
  const { narrow } = this;
  return html`<my-component ?narrow="${narrow}">
    demo content
  </my-component>`;
}
```

The **dark theme** should be applied by setting up styles for `body.styled.dark` CSS selector in the demo's HTML file. When the user toggle the view this styles will be applied.

```css
body.styled.dark {
  ...
}
```

Finally, the **toggle styles** option toggles the `styled` class from the body element. It then allows to preview the element with the default styling.



