---
title: RequireJS
page_title: RequireJS | Kendo UI Third-Party Tools
description: "Learn how to work with Kendo UI and RequireJS compatible loader."
previous_url: /using-kendo-with-requirejs
slug: requirejs_integration_kendoui
position: 4
---

# RequireJS

>**Important**
> * As of 2016, the RequireJS project is mostly superseded by solutions like [Webpack](http://webpack.github.io/), [Browserify](http://browserify.org/) and [SystemJS](https://github.com/systemjs/systemjs), which provide much more extensible API.
> * You may check the help articles on their integration too&mdash;[Webpack support]({% slug webpacksupport_integration_kendoui %}) and [SystemJS support]({% slug systemjs_integration_kendoui %}).
> * Due to a bug, the examples below do not work with the official Kendo UI Q1 2016 release. They should work as expected with the versions 2016.1.118 and later.

The minified Kendo UI JavaScript files are [AMD modules](https://en.wikipedia.org/wiki/Asynchronous_module_definition) and work with compatible loaders such as [RequireJS](http://requirejs.org/). You can use this feature to load only the needed Kendo UI JavaScript files instead of `kendo.all.min.js`.

## Load Kendo UI Using RequireJS

### Load Kendo UI from Local Directory

The example below demonstrates how to load the Kendo UI JavaScript files from a local directory, e.g. `js/kendo`.

###### Example

```html
<!DOCTYPE HTML>
<html>
    <head>
        <link rel="stylesheet" href="../dist/styles/web/kendo.common-material.core.min.css">
        <link rel="stylesheet" href="../dist/styles/web/kendo.material.min.css">
        <!-- Include RequireJS -->
        <script src="http://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.1/require.js"></script>
    </head>
    <body>
        <select id="dropdownlist"></select>
        <script>
        require.config({
          baseUrl: "js/kendo", // the path where the kendo scripts are present
          paths: {
            "jquery": "http://code.jquery.com/jquery-1.9.1.min",
          }
        });

        require([ "jquery", "kendo.dropdownlist.min" ], function($, kendo) {
          console.log(kendo)

          $("#dropdownlist").kendoDropDownList({
            dataSource: {
              data: [{name:"Jane Doe", value: 1}, {name:"John Doe", value: 2}]
            },
            dataTextField: "name",
            dataValueField: "value"
          });
        });
        </script>
    </body>
</html>
```

### Use Bundle Scripts with RequireJS

The example below demonstrates how to use a bundle script with RequireJS.

###### Example

```html
<!DOCTYPE HTML>
<html>
    <head>
        <link rel="stylesheet" href="../dist/styles/web/kendo.common-material.core.min.css">
        <link rel="stylesheet" href="../dist/styles/web/kendo.material.min.css">
        <script src="http://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.1/require.js"></script>
    </head>
    <body>
        <select id="dropdownlist"></select>
        <script>
        require.config({
          paths: {
            "jquery": "http://code.jquery.com/jquery-1.9.1.min",
            "kendo.ui.core.min": "js/kendo/kendo.ui.core.min" // the name of the module should match the file name - paths won't work.
         }
        });

        require([ "jquery", "kendo.ui.core.min" ], function($, kendo) {
          $("#dropdownlist").kendoDropDownList({
            dataSource: {
              data: [{name:"Jane Doe", value: 1}, {name:"John Doe", value: 2}]
            },
            dataTextField: "name",
            dataValueField: "value"
          });
        });
        </script>
    </body>
</html>
```

### Use AngularJS and Kendo UI with RequireJS

The example below demonstrates how to load AngularJS and initialize it with [`angular.bootsrap`](https://docs.angularjs.org/api/ng/function/angular.bootstrap) when all `.js` files are loaded.

###### Example

```html
<!DOCTYPE HTML>
<html>
    <head>
        <link rel="stylesheet" href="../dist/styles/web/kendo.common-material.core.min.css">
        <link rel="stylesheet" href="../dist/styles/web/kendo.material.min.css">
        <script src="http://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.1/require.js"></script>
    </head>
    <body>
        <div ng-controller="controller">
          <select kendo-drop-down-list k-options="options"></select>
        </div>

        <script>
        require.config({
          paths: {
            "angular": "https://ajax.googleapis.com/ajax/libs/angularjs/1.3.12/angular.min",
            "jquery": "http://code.jquery.com/jquery-1.9.1.min",
            "kendo.ui.core.min": "../dist/js/kendo.ui.core.min"
          },
          shim: {
            "angular": { deps: ["jquery"] },
            "kendo.ui.core.min": { deps: ["angular"] }
          }
        });

        require([ "angular", "kendo.ui.core.min" ], function() {
            var app = angular.module("app", ["kendo.directives"]);

            app.controller("controller", ["$scope", function($scope) {
              $scope.options = {
                dataSource: {
                  data: [{name:"Jane Doe", value: 1}, {name:"John Doe", value: 2}]
                },
                dataTextField: "name",
                dataValueField: "value"
              };
            }]);

            angular.bootstrap(document, ["app"]);
        });
        </script>
    </body>
</html>
```

## See Also

Other articles on Kendo UI integration with third-party tools and frameworks:

* [Twitter Bootstrap]({% slug twitterbootstrapintegration_integration_kendoui %})
* [Web Components]({% slug webcomponents_integration_kendoui %})
* [Angular 2.0]({% slug angular2support_integration_kendoui %})
* [TypeScript]({% slug typescript_integration_kendoui %})
* [Visual Studio IntelliSense]({% slug visualstudiointellisense_integration_kendoui %})
* [Telerik Data Access]({% slug bindtotelerikdataaccesstool_integration_kendoui %})
* [SystemJS Support]({% slug systemjs_integration_kendoui %})
* [Webpack Support]({% slug webpacksupport_integration_kendoui %})
