# Building an API demo page

To build an API related demo page use `ApiDemoPage` class as a base class for the demo page. It extends `DemoPage` class but adds additional UI and logic into it. The `ApiDemoPage` adds the following logic and UI:

* A drop down to select an API \(`demo-api` by default\),  
* Logic to load API models
* API navigation view
* Navigation selection handling

On top of the configuration options available for `DemoPage` and overriding view generation method you should also override `_navChanged()` method that is called when a navigation occurs. 

## Minimal example

```javascript
import { html } from 'lit-html';
import { ApiDemoPage } from '@advanced-rest-client/arc-demo-helper';

class ApiDemo extends ApiDemoPage {
  constructor() {
    super();
    this.componentName = 'api-my-component';
    this.renderViewControls = true;
    this.initObservableProperties([
      'amfShape'
    ]);
  }
  
  // override this to handle navigation change
  _navChanged(e) {
    const { selected, type } = e.detail;
    // process selection information somehow
    this.amfShape = this.getAmfShape(selected, type);
  }

  contentTemplate() {
    const { amf, amfShape } = this;
    return html`<api-my-component .amf="${amf}" .shape="${amfShape}">
      This is demo page for API components
    </api-my-component>`;
  }
}

const instance = new ApiDemo();
instance.render();
```

## Adding API definition

All APIs has to be located in `demo/` folder. By default the demo page will try to load the `demo-api.json` file at page load. This can be changed by extending `_apiListTemplate()` method. It should return a template of `<anypoint-item>` with `data-src` property that contains a name of a file to load.

```javascript
class ApiDemo extends ApiDemoPage {
  _apiListTemplate() {
    return html`
    <anypoint-item data-src="my-api.json">Demo api</anypoint-item>
    <anypoint-item data-src="my-api-compact.json">Demo api - compact version</anypoint-item>`;
  }
}
```

This renders two options in the APIs drop down list. When a selection change the demo page loads the API from the current directory and sets the `amf` property that is propagated to the view. By default the first item on the list is loaded with page load.

{% hint style="info" %}
There is no common API for all components as they should focus on specific demo. You should defined RAML and OAS APIs to your demo.
{% endhint %}

## Generating API models

Use `@api-components/api-model-generator` module to generate the models using \(most often\) the last version of AMF.

The first step is to install the library as a dev dependency of the project.

```bash
npm i -D @api-components/api-model-generator
```

Then add all `.json` files in the demo page to the list of git ignored files.

```text
/demo/*.json
!demo/apis.json
```

The second line represents a list of files to process when generating a model. This is an example of such `apis.json` file:

```javascript
{
  "demo-api/demo-api.raml": {"type": "RAML 1.0", "mime": "application/yaml+raml", "resolution": "editing"}
}
```

The APIs definition file must contain a map of APIs where the key is the path to the file and the value is an object that defines processing options:

* **type** \(required\) API type: `RAML 1.0`, `RAML 0.8`, `OAS 3.0`, `OAS 2.0`
* **mime** \(optional\) API mime. Defaults to `application/yaml+raml`
* **resolution** \(optional\) AMF resolution pipeline. Defaults to `editing`

Then add `demo/model.js` file with the following content:

```javascript
const generator = require('@api-components/api-model-generator');
generator('./demo/apis.json')
.then(() => console.log('Models created'))
.catch((cause) => console.error(cause));
```

{% hint style="info" %}
Don't change file names. They are used by the CI to generate tests. 
{% endhint %}

Add a command to the `package.json` to generate models when the component is being installed:

```javascript
"scripts": {
  "prepare": "node demo/model.js"
}
```

After running `npm i` the models are generated for the APIs defined in the demo folder.

