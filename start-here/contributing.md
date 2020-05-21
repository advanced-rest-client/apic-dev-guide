# Contributing

## Guide for Contributors

The Advanced REST Client, ARC Components, ans API Components are open and we encourage the community to contribute in the project. However, it is very important to follow couple of simple rules when you create an issue report or send a pull request.

### Issue reporting

#### Feature request

Please provide a clear description of the feature. I've created a [document](https://docs.google.com/document/d/10OPWl9Hagk6Oz--VUztQBTOpm3QP2Vv__PrH3zZ7wFQ/edit?usp=sharing) that may help you fill up a feature request. But basically you should answer this questions:

* Who will benefit from this feature? \("_Someone who is trying to..._"\)
* What's the use cases \(when the feature will be used\)? \(_"When the HTTP response is..."_\)
* What the user gain by having this feature? \(_"I should be able to see..."_\)

An overview of the feature would be nice as well.

#### Bug report

Please be as much specific as you can and add a clear description of the bug, logs if available and your expectations. You're welcome to use following template:

```text
The send button causes the app to close itself.

## Expected outcome
The app is not closing itself when I try to send a request.

## Actual outcome
After click it is working for couple of second and the it closes itself.

## Versions
Component/app version: x.x.x
Browser: Chrome, stable (or) 47.0.111.111 (check it in Chrome's about page)

# Steps to reproduce
1. Turn off CodeMirror
2. Set method to "PUT"
3. Use "raw" tab for both payload and headers
4. Paste following data into the headers field: ...
5. Paste following data into the payload field: ...
6. Run the request.
```

### Submitting Pull Requests

While developing, be sure that you follow the design guidelines described in this section of the documentation.

Then a good idea is to test your code. It will save all of us a lot of trouble.

Please ensure that an issue exists for the corresponding change in the pull request that you intend to make. If an issue does not exist, please create one per the guidelines above. The goal is to discuss the design and necessity of the proposed change with ARC authors and community before diving into a pull request.

When submitting pull requests, please provide:

**A reference to the corresponding issue** or issues that will be closed by the pull request. Please refer to these issues using the following syntax:

```text
(For a single issue)
Fixes #20

(For multiple issues)
Fixes #32, #40
```

Github automatically close the issues after merging it to the master. So please, keep the format.

**A succinct description of the design** used to fix any related issues. For example:

```text
This fixes #20 by removing styles that leaked which would cause the page to turn pink whenever `api-foo` is clicked.
```

At least one test for each bug fixed or feature added as part of the pull request. Pull requests that fix bugs or add features without accompanying tests will not be considered.

If a proposed change contains multiple commits, please squash commits to as few as is necessary to succinctly express the change.

