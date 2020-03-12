# Styling the demo page

API Components uses CSS variables to apply styling internally. To style a component this variables should be used. To demo styling options each demo page should include styling for light \(default\) and dark theme. A developer should build a demo that support dark mode to ensure that all required variables are set in the template.

The `DemoPage` class sets `styled` attribute on the body after initialization. All styles should be applied to this CSS selector so it can be turned off when the user disable styling in the view options. The styles should be applied to make the demo page more accessible for the consumers. The components itself should include minimal amount of own styling and use variables instead. This way we can ensure that a different styling configuration can be applied to the component.

```markup
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, minimum-scale=1, initial-scale=1, user-scalable=yes">
    <title>Demo page</title>
    <style>
    body.styled {
      --primary-color: blue;
    }
    
    body.styled.dark {
      --primary-color: white;
    }
    </style>
  </head>
  <body class="styled">
    <div id="demo"></div>

    <script type="module" src="./index.js"></script>
  </body>
</html>
```

