---
title: Introduction
page_title: Documentation guide for Telerik UI for PHP
previous_url: /tutorials/PHP/build-apps-with-kendo-ui-and-php, /tutorials/PHP/build-apps-with-kendo-ui-and-php-2
description: How to download, install Telerik UI for PHP and run the sample application.
---

# Introduction

Telerik UI for PHP is a set of PHP classes which help you configure Kendo UI widgets by using server-side code in PHP web sites.

## Requirements

Telerik UI for PHP requires PHP 5.3+. The sample application uses [PDO](http://www.php.net/manual/en/intro.pdo.php)
and [SQLite](http://www.php.net/manual/en/ref.pdo-sqlite.php).
Both extensions should be enabled in the PHP configuration (php.ini). The
[phpinfo](http://php.net/manual/en/function.phpinfo.php) function can be used to verify that PDO and the SQLite extensions are successfully installed.

## Download and Install Telerik UI for PHP

You can download Telerik UI for PHP from the [official download page](http://demos.telerik.com/php-ui/). The distribution zip file
contains the following directories:

*   **/js** - minified JavaScript files.
*   **/styles** - minified CSS files and background images used by the themes.
*   **/src** - JavaScript and CSS source files. Not available in the trial version.
*   **/wrappers/php/lib/Kendo/** - PHP files required to use Telerik UI for PHP.
*   **/wrappers/php/** - sample PHP web site.

## Run the Sample Website

You can find a sample PHP web site in the **/wrappers/php/** directory of the Telerik UI for PHP distribution.
To run the web site copy this directory to your web root. Then navigate to **index.php**.

## Use Telerik UI for PHP in Your PHP Website

1. Copy **/wrappers/php/lib/Kendo** to your web site root (for example in your **lib** directory).
2. Copy the Kendo UI JavaScript and CSS files from **/js** and **/styles** to your web site root. If you prefer to use [Kendo UI CDN Service](/intro/installation/cdn-service) skip this and the next steps
and check the [JavaScript Dependencies](/intro/installation/prerequisites) help topic.
3. Include the Kendo UI JavaScript and CSS files in your PHP page:

        <link href="styles/kendo.common.min.css" rel="stylesheet" type="text/css" />
        <link href="styles/kendo.default.min.css" rel="stylesheet" type="text/css" />
        <script src="js/jquery.min.js"></script>
        <script src="js/kendo.web.min.js"></script>

    > **Important**  
    > If you want to use Kendo UI Web and DataViz at the same time, you should include **kendo.all.min.js** instead of **kendo.web.min.js** and **kendo.dataviz.min.js**. You can also create a custom JavaScript file using the [Custom Download Builder](https://www.telerik.com/login/v2/telerik?ReturnUrl=https://www.telerik.com/download/custom-download).

4. Include the Kendo PHP [Autoload](http://php.net/manual/en/language.oop5.autoload.php) file.

        <?php require_once 'lib/Kendo/Autoload.php'; ?>

5. Use any Kendo UI PHP wrapper:

        <?php
        // Instantiate a new instance of the DatePicker class and specify its 'id'
        $datepicker = new \Kendo\UI\DatePicker('datepicker');

        // Configure the datepicker using the fluent API
        $datepicker->start('year')
                   ->format('MMMM yyyy');

        // Output the datepicker HTML and JavaScript by echo-ing the result of the render method
        echo $datepicker->render();
        ?>

###### Example - the complete source

    <!DOCTYPE html>
    <html>
        <head>
            <link href="styles/kendo.common.min.css" rel="stylesheet" type="text/css" />
            <link href="styles/kendo.default.min.css" rel="stylesheet" type="text/css" />
            <script src="js/jquery.min.js"></script>
            <script src="js/kendo.web.min.js"></script>
        </head>
        <body>
        <?php require_once 'lib/Kendo/Autoload.php'; ?>
        <?php
        // Instantiate a new instance of the DatePicker class and specify its 'id'
        $datepicker = new \Kendo\UI\DatePicker('datepicker');

        // Configure the datepicker using the fluent API
        $datepicker->start('year')
                   ->format('MMMM yyyy');

        // Output the datepicker HTML and JavaScript by echo-ing the result of the render method
        echo $datepicker->render();
        ?>
        </body>
    </html>

## Next Step

Watch the videos in the [Kendo UI YouTube channel](http://www.youtube.com/kendouitv):

1. [Getting Started With The PHP Wrappers For Kendo UI: Part 1](http://www.youtube.com/watch?v=2Kpgp_nocEI)
1. [Getting Started With The PHP Wrappers For Kendo UI: Part 2](https://www.youtube.com/watch?v=RQCLqA6Pu_E)
1. [Getting Started With The PHP Wrappers For Kendo UI: Part 3](https://www.youtube.com/watch?v=zYh6cuU_leQ)

For more examples on how to use Kendo UI with PHP wrappers, visit the following GitHub repository:

* [Collected Kendo UI Examples on Using PHP](https://github.com/telerik/kendo-examples-php)
