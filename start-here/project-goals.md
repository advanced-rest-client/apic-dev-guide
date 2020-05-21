# Project goals

The actual order is not important. A number next to the goal name is used just to reference the rule in this documentation.

## \#1 Interoperability

The components are designed to maximize interoperability and to reduce custom code while using the components in different environments. The components vale to work in any web environment: web platform, electron, or similar application. This also includes build tools. The components are designed to enable use of any web build tools that understand custom elements.

## \#2 Performance

The components are not an application, they are part of a bigger thing. It is important to remember that a single component can't impact performance in any way. Developers who will use the components in their projects will expect that it will not impact overall performance of their application. Performance is defined by the following metrics:

* computation time \(less CPU cycles the better\)
* memory usage \(less memory used the better\)
* code size \(the application should run instantly on a mobile device on a 3G network\)

## \#3 Accessibility

The components are accessible by default. They comply with WCAG guidelines and are tested against them. The components respect text direction \(LTR, RTL\). Mobile device access is also an accessibility issue. This goal should be understood as to remove any barrier that prevents a user from accessing the content whatever the use context is.

## \#4 Security

API Components operates on API specification model and often interact with the data received by the API or entered by the user. This probably includes protected or private information. The components are build in respect of users and organization privacy.

