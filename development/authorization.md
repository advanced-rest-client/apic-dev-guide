---
description: This page describe how to handle authorization in API Components / API Console
---

# Authorization

{% hint style="info" %}
This document describes authorization supported by authorization-method, api-authorization-method, and api-authorization. This is used in api-request-editor/panel &gt;= 5.0.0.
{% endhint %}

The base class \(component\) for authorization method is included into `@advanced-rest-client/authorization-method` component. It provides UI for popular authorization methods. The `@advanced-rest-client/api-authorization-method` adds support for AMF model processing. This enables application to pass security definition to the authorization method and it automatically sets defaults from the model. Finally `@advanced-rest-client/api-authorization` is a components that renders authorization method\(s\) depending on defined in the AMF model security schemes and user selection.

## Handling authorization in API Console \(element\)

Most authorization methods that operates on request properties rather than the HTTP connection are handled by the API Console element out of the box. This includes:

* Basic
* OAuth 2
* OAuth 1
* Custom scheme \(RAML\)
* Api Key \(OAS\)
* Bearer \(OAS\)

For the methods mentioned above no development is required. However, **Digest** method requires interaction with the server on the connection level. This has to be handled by the hosting application. Normally a browser would handle this authorization so no additional work is required. But in case of a proxy or mocking service the server application should handle authorization. This can be done by reading value of `auth` property of the request object.

## The "auth" property on API request

The request object contains `auth` property of authorization is ever defined for current operation. It contains an array of `AuthConfig`. This array is created by calling `serialize()` function on the api-authorization element.

```text
// AuthConfig
{
  "valid": boolean,
  "type": string,
  "settings": object
}
```

All properties are always populated. The `valid` property tells whether current configuration is considered valid. The `type` property is the type of the `api-authorization-method` element \(it's type attribute\): basic, digest, pass through, oauth 1, oauth 2, api key, bearer, and custom.  
The `settings` property contains a settings for the authorization method. The settings vary depending on the type.

To find a particular authorization settings you can use the `find()` function.

```text
const digest = element.auth.find((item) => item.type === 'digest');
```

Then the digest settings \(if any\) can be used with the application to initialize the request.

## Authorization handling via api-authorization, api-request-editor, api-request-panel

OAuth 1 and OAuth 2 token requests are handled internally by the `api-request-editor` element as the `@advanced-rest-client/oauth-authorization` element is used in the internal DOM. However, the auth logic components \(`aouth1-authorization` and `oauth2-authorization`\) listens on the `eventsTarget` for the events. By default it is the `window` object. It can be set to any parent element. This gives you an option to handle the `oauth2-token-requested` event on any of the `api-*` elements. Check [advanced-rest-client/oauth-authorization](https://github.com/advanced-rest-client/oauth-authorization) for example implementation.

