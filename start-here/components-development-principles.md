---
description: >-
  This page describes the basic principles of API Components development. This
  should be treated as a guidelines for development. Understanding the
  principles will help you understand the architecture.
---

# Components development principles

**Web Standards**

The components were developed to always comply with web standards. Any abstraction is limited to absolute minimum. This was every web developer has less to learn before starting development of a component. This is also why the components uses LitElement. It gives almost none abstraction and the most flexibility.

Web standards also support goal \#1 - **interoperability**. By reducing abstraction and custom syntax the components can be used with any build system that understands what a custom element is.

**Performance**

This directly refers to goal \#2. While developing a component bear in mind that it will become a part of another application. Think of [API Console](https://github.com/mulesoft/api-console). It is an application by itself but it can be also embedded in another application. It is used as a part of Anypoint API Designer. The components are also used in other applications like Anypoint Exchange or Advanced Rest Client. The consumers should not notice decrease in their application performance. It is not only about the speed and memory use. Every byte counts. The application code has to be downloaded, parsed, and executed instantly. 

MuleSoft has no performance budget set when it comes to metrics like critical path download size, first contentful paint, or time to interactive. Because of that we have to be as fast as possible. To do it we need to have no external libraries if really necessary approach. It's easy to load another library into a component, use 5% of it's functionality, and waste the rest of the transfer. Tree shaking is good but not 100% accurate in some cases.

**Accessibility**

Both Salesforce and MuleSoft builds for accessibility. Our software must be consumable regardless of user background and context. Components must comply with Web Content Accessibility Guidelines \(WCAG\). All components that contains focusable or otherwise interactive UI regions must include a11y tests. However, accessibility is not only screen reader for visually impaired people. It's ability to render text right-to-left for some languages like Arabic, Hebrew, and so on. It is also being accessible on touch screen only when an application run in a kiosk mode. Finally it is also access on mobile devices. If we fail in any of these areas we fail with providing accessible software in general.

**Standards in development**

In a distributed projects like API Components it's very easy to mix up several coding styles, have different commit conventions, different tooling. This is OK if a software is being built by one person or a team for own consumption. But it doesn't work with cross-team and cross-application environment. It is important to use standard tooling, style, linting, and generally the same tech stack. This way a developer can maintain a component built by another developer from the other team. Take a look into [Code style](code-style.md) section for more information.

**Designing component's API**

It is easy to design an API that uses complex data structures for configuration. But then you should ask yourself a question how often such configuration is used in the native web platform? The answer is never. Instead of passing a configuration object that configures the behavior of an element web authors composite elements together. To understand this consider the `<video>` element and the options to configure its behavior. Simple configuration options \(the ones that uses primitives as values\) are attributes on the video tag. Complex structures are represented by other HTML elements that are placed as a children of the `<video>` tag. The `<source>` element configures the list of video files to be played and the UA can choose which one to play depending on current view port and support for codecs. Similarly  the `<track>` element configures the list of captions available for the video.

Make sure you are familiar with [web components gold standards](https://github.com/webcomponents/gold-standard/wiki) prepared by Salesforce architect for LWC Jan Miksovsky. It's the best so far compilation of general design principles for web components.

