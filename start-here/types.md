# Types

The components are JavaScript project but TS can be used through a declaration files. Previously ARC and API components were using Polymer's typescript generator but it is not ideal. Because of that types declaration files should be made manually to ensure quality.

Take a look into a typical types declaration file made from a component definition \(it uses mixins!\) [AuthorizationMethod.d.ts](https://github.com/advanced-rest-client/authorization-method/blob/stage/src/AuthorizationMethod.d.ts). Mixins are described differently. Take a look into [Oauth2MethodMixin.d.ts](https://github.com/advanced-rest-client/authorization-method/blob/stage/src/Oauth2MethodMixin.d.ts) file for an example. When you are making a mixin that mixes in another mixin then use this pattern described in [MenubarMixin.d.ts](https://github.com/anypoint-web-components/anypoint-menu-mixin/blob/stage/src/MenubarMixin.d.ts) file. Note that the mixin function declaration extends the other mixin.

Finally don't forget to associate an HTML tag with a TS declaration. You can check this example in [anypoint-selector.d.ts](https://github.com/anypoint-web-components/anypoint-selector/blob/stage/anypoint-selector.d.ts) file.

