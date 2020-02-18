# API Components architecture overview

## Composition

### Web composition model

In API Components ecosystem there are 2 types of components: base components and composite components.

**Base components** are the most basic parts of the UI. They usually don't contain other custom elements. They are used to build composite components.

An example of such component is `<anypoint-chip>` \([anypoint-chip](https://www.npmjs.com/package/@anypoint-web-components/anypoint-chip) docs\). It is a component to render a chip styled with Material Design. It can be used as is but it has been created to support other elements when building filters, options, actions, and chip inputs.

**Composite components** are complex components build on top of existing components. You can think of it as parts of a more complex base components or a UI regions of the application.

An example of composite component is `<anypoint-chip-input>` \([anypoint-chip-input](https://www.npmjs.com/package/@anypoint-web-components/anypoint-chip-input) docs\). It uses `<anypoint-input>` to render the input and then `<anypoint-chip>` to render input values. It adds some own logic to support autocomplete feature.

The `<anypoint-chip-input>` element is ready to be used in a web application but also it can be used in other components. Imagine a search bar with filter options. You would use the `<anypoint-chip-input>` element to render input for filters like tags or types. This search bar would become another composite component.

What is important is not to assume that your component will work with a specific other component. In the web environment it is almost never happens. Take a dropdown menu as an example.

```text
<anypoint-dropdown>
  <anypoint-listbox>
    <anypoint-item></anypoint-item>
  </anypoint-listbox>
</anypoint-dropdown>
```

The `anypoint-dropdown` element assumes it will have a child that exposes a `selected` property that is used to determine current selection and therefore it can obtain corresponding label to render when the `dropdown` is closed. The `listbox` element just renders whatever was added to it's [light DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom#lightdom) and manages selection state. It doesn't really matter for the components what is the name of the component passed as a child element. The `anypoint-listbox` renders `anypoint-item` but it will also render `img` or `p` elements as well. The same is for `anypoint-dropdown` element. As long as child element has the `selected` property it will work as expected. However, if the element won't expose this property the component should still render content without any error. This is consistent with the web platform. When a `p` \(paragraph\) is passed to the `ul` \(unordered list\), the paragraph will still be rendered even though the composition is incorrect. The component should not throw error when incorrectly initialized.

### Composite API components

API Components comes with a rich library of web components that allows developers to build complex application for interacting with APIs. Each component reuse existing components and minimize code repetition. Basic rule is to create a separate component or a JavaScript mixin that can be reused later in other components.

The component never serves more than one purpose. In the example above the `anypoint-dropdown` only renders a label for the dropdown form control and changes the label when selection in the `anypoint-listbox` element changes. Selection management is a sole responsibility of `anypoint-listbox` element. It has `selected` and `selectedItem` properties and few helper functions like `selectNext()` or `clearSelction()` functions. Finally `anypoint-item` just renders any content passed to the [light DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom#lightdom) in a single line. Having this composition you can:

* reuse `anypoint-listbox` in various situations when a selection of one of the options is required, like application menus, drop downs, radio buttons and so on.
* reuse `anypoint-item` whenever you need to render a list if items in an unified way like menus, drop downs, selection options.

This kind of composition is highly scalable when creating new components. Instead of repeating code that serves the same purpose but in different context, it uses another component.

#### Things to avoid

**Don't build monoliths**. The `my-dropdown` components could be build as a single component that accepts a configurable list of children to render. This, however, **is not scalable** as the state selection and rendering is enclosed in this component and cannot be shared. It only allows you to render very specific view because the implementation of rendering of the list items is very specific to this component. There is no way to render in the dropdown a list of images without breaking changes to the component. Finally it is not web friendly as this is not how native HTML elements work.

**Think about the web the web way**. When creating new component think how similar patterns were used in the web platform. When using similar and therefore familiar patterns the developer that uses the component in the future has less work to do when learning how to use the component.

## Declarative vs imperative

Some of the components in the API Components library do not offer an UI. We call them logic components. They only performs some kind of logic but have no visual layer. You may think a JavaScript library should be used instead and in many use cases you would be right. However, sometimes the component may need to use DOM APIs and it would be inconvenient to initialize the functionality via imperative API. API Components prefer declarative use of APIs.

`arc-models` elements are an example of this. The `arc-models` elements is a set of components that provide an access to the datastore in Advanced REST Client. The [request-model](https://github.com/advanced-rest-client/arc-models/blob/3.0.0-preview/request-model.js) component provides access to the request store and allows to query for requests for given criteria. It could be a JavaScript library but the model listens to DOM events so other components requesting access to the datastore do not have to have direct access to the model element. When `request-model` is inserted to the DOM it attaches a listener to a document node and waits for events or direct call of a function in exposed API. It also dispatches events when a model changes so other elements can react on the change. It could be done with a library but with each component that uses the model you would have to initialize the library and remember to clean up when the component is detached from the DOM. Finally, as mentioned, in API Components ecosystem, we prefer declarative programming. Therefore the model should be added to the shadow DOM of the component or application instead of calling a function in JavaScript library.

This doesn't apply to 3rd party libraries which has to be used imperatively unless a custom element wrapper is created for the library.

### Why to use declarative APIs

**Semantics.** An HTML tag used in the DOM has own semantic and it is easier to understand what the application does.

**Events API** This enables event based communication between components. Instead of initializing the same component multiple times inside the same application it is more performant to use a single component that listens for DOM events, performs an operation, and dispatches an event when the work is done.

**Familiarity** This is well known pattern in web programming. Declare an HTML element in the DOM and use JavaScript to handle events or to call API functions. Each web developer understands this and therefore it is easier to adjust.

## Passing data to and from a component

**Use attributes to receive non-complex data**. Elements are configured declaratively as much as possible, which allows them to be used declaratively, as well as work great with template bindings. For complex data structures use JavaScript properties instead. Properties for complex data structures do not have to be reflected to an attribute. This is how the native elements works. You can control their behaviour using both JavaScript properties and attributes.

**Use events to pass data to the outside world**. All custom elements inherit from EventTarget, and thus events can be used to pass data along, but please follow the guidelines in the [DOM Living Standard §2.11](https://dom.spec.whatwg.org/#action-versus-occurance). See also the TAG’s guidance on [event design](https://w3ctag.github.io/design-principles/#event-design).

## Events API

See guidance on [event design](https://w3ctag.github.io/design-principles/#event-design) as it helps to understand how web events are designed.

API components has events API that allows the components to communicate with each other without being directly "wired" together by a binding system. At the first iteration of the components library elements were bind together via Polymer's binding system. This caused a lot of problems when it comes to re-usability. Consider the content type selector and the headers editor used in HTTP request editor UI. Both of them are operating on the headers string that is used to construct HTTP request. However, they are not used in the same part of the application. The headers editor is included into request editor and the content type selector is included in the body editor \(code editor exactly\). Naturally in this case data binding would work as expected. However, you would have to do the same thing every time you want to use this two elements in other application in different configuration. It is inconvenient for an author to wire components like this and it has to be somehow documented in both components.

Instead API components has a common communication system so this two components can communicate a change by using events system. When one dispatches an event about `content-type` header change then the other updates its state without being connected to each other. The same model is used in `arc-models`. When a model has changed and the changes has been committed to the data store the component dispatches corresponding event. Each component that relays on the model state listens for this event and updates its state if necessary. Consider `arc-history-menu` element. When new entry is added to the history store the menu should also add new item to the list. This is done by listening for change event dispatched by the model.

Finally, events API allows to easily replace one component with another when platform APIs requires different implementation \(web vs Electron\). As long as the replacement support the same event's it still can work with other components without refactoring major part of the application.

Currently there is no central documentation for events API. Each event is documented in the component documentation. It may be available in the future. Look for documentation parts in the code with `@event` tag. See an example at [https://github.com/advanced-rest-client/arc-models/blob/3.0.0-preview/request-model.js\#L1360](https://github.com/advanced-rest-client/arc-dev-guide/blob/master/docs).

## Shell application concept

Both API Console and Advanced REST Client follow the same pattern when building an application. It is a shell application. The application itself is a host that contains all components needed for the functionality of the application and provide platform specific interfaces \(usually via events API\). In this model the application do not provide application own logic as it should be enclosed in the components for reusability.

Take API Console as an example. The application consist of three main components:

* `api-navigation`
* `api-documentation`
* `api-request-panel`

This three components are responsible for rendering navigation and documentation. They are also making a test request to currently selected method of an endpoint.

The logic of the application is enclosed in those three components and the `api-console` application only passes navigation selection state to the other components. It also manages state for media queries and allow to load AMF model from an external file. However, it has nothing to do with rendering navigation, documentation, or making a request to an endpoint.

With this pattern it is relatively easy to introduce new application on different platform. It would be easy to create API Console as an Electron or Chrome application just by providing different shells. The application logic is still the same - 3 components working together.

One note here. Sometimes your code may require platform specific API. For example, `localStorage` is not available in Chrome Apps. You have to use either `IndexedDB` or Chrome specific API. In this situation it is better to use events to store / read data from `localStorage` and introduce a component that handles the events. In case of the chrome apps platform you only have to replace the storage component instead of the underlying component which would cause significant code refactoring and reduce time to market with new application.

## Accessibility

Accessibility is very important for custom elements. It is crucial to add accessibility attributes and states to your component. There are multiple contexts the user can use your component. It can be visually impaired person that uses spoken feedback, it can be a kiosk application that has only keyboard available, or high contrast environment.

The component has to use proper `role` and `aria-*` attributes to ensure the component can be used in all this situations. Components without accessibility setup won't be accepted to the API Components ecosystem. Also, each component is required to contain accessibility tests. Even though automatic tests can only discover handful number of problems it is still better to use them than not at all.

Further reading:

* [Web Components design guidelines](https://www.w3.org/2001/tag/doc/webcomponents-design-guidelines/)
* [Web Components Gold Standard](https://github.com/webcomponents/gold-standard/wiki)



