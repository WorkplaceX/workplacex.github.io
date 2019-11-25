---
title: Embed WorkplaceX Application
--- 

# Embed WorkplaceX Application
If you have an existing company web site you can easily embed a WorkplaceX application. Just add a few lines of code.

There is two options of doing it:
## Use the Html embed tag
Add the following line of code
```html
<embed src="https://demo.workplacex.org">
```

## Use Ccss and Scripts Directly
Alternatively you can use the css and scripts directly in your company web site. The minimum implementation looks like this:
```html
<html>

<head>
  <!-- WorkplaceX Framework css-->
  <link rel="stylesheet" href="https://demo.workplacex.org/frameworkStyle.css">
</head>

<body>
  <!-- WorkplaceX Application-->
  <app-root></app-root>
  <script>
    var jsonBrowser = { "EmbeddedUrl": "https://demo.workplacex.org/" };
  </script>

  <!-- WorkplaceX Angular -->
  <script src="https://demo.workplacex.org/runtime-es2015.js" type="module"></script>
  <script src="https://demo.workplacex.org/runtime-es5.js" nomodule defer></script>
  <script src="https://demo.workplacex.org/polyfills-es5.js" nomodule defer></script>
  <script src="https://demo.workplacex.org/polyfills-es2015.js" type="module"></script>
  <script src="https://demo.workplacex.org/main-es2015.js" type="module"></script>
  <script src="https://demo.workplacex.org/main-es5.js" nomodule defer></script>
  <script type="text/javascript" src="https://demo.workplacex.org/bundle.js"></script>
</body>

</html
```
The app-root html tag is where the application is rendered in your company web site.
