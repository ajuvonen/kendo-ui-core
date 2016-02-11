---
title: Overview
page_title: How to use ComboBox HtmlHelper extension | Kendo UI documentation
description: User Guide for server-side wrapper for Kendo UI ComboBox for ASP.NET MVC widget.
---

# ComboBox

The ComboBox HtmlHelper extension is a server-side wrapper for the [Kendo UI ComboBox](/api/javascript/ui/combobox) widget.

## Getting Started

There are two ways to bind a Kendo ComboBox for ASP.NET MVC:

*   **server** - the data will be serialized to the client. No Ajax requests will be made.
*   **ajax** - the combobox will make ajax request to get the data.

### Configure ComboBox for Server Binding

Here is how to configure the Kendo ComboBox for server binding to the Northwind Products table using Linq to SQL:

1.  Make sure you have followed all the steps from the [Introduction](/aspnet-mvc/introduction) help topic.

2.  Create a new action method and pass the Products table as the model:

        public ActionResult Index()
        {
            NorthwindDataContext northwind = new NorthwindDataContext();

            return View(northwind.Products);
        }
3.  Make your view strongly typed:
    - WebForms

            <%@ Page Language="C#" MasterPageFile="~/Views/Shared/Site.Master"
               Inherits="System.Web.Mvc.ViewPage<IEnumerable<MvcApplication1.Models.Product>>" %>
    - Razor

            @model IEnumerable<MvcApplication1.Models.Product>
4.  Add a server bound combobox:
    - WebForms

            <%: Html.Kendo().ComboBox()
                .Name("productComboBox") //The name of the combobox is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
                .BindTo(Model)   //Pass the list of Products to the combobox.
                .SelectedIndex(10) //Select an item with index 10. Note that the indexes are zero based.
            %>
    - Razor

            @(Html.Kendo().ComboBox()
              .Name("productComboBox") //The name of the combobox is mandatory. It specifies the "id" attribute of the widget.
              .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
              .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
              .BindTo(Model)   //Pass the list of Products to the combobox.
              .SelectedIndex(10) //Select an item with index 10. Note that the indexes are zero based.
            )

### Configure ComboBox for Ajax Binding

Here is how to configure the Kendo ComboBox for ajax binding to the Northwind Products table using Linq to SQL:

1.  Make sure you have followed all the steps from the [Introduction](/aspnet-mvc/introduction) help topic.

2.  Create an action method which renders the view:

        public ActionResult Index()
        {
            return View();
        }
3.  Create a new action method and pass the Products table as Json result:

        public JsonResult GetProducts()
        {
            NorthwindDataContext northwind = new NorthwindDataContext();

            return Json(northwind.Products, JsonRequestBehavior.AllowGet);
        }
4.  Add an ajax bound combobox:
    - WebForms

            <%: Html.Kendo().ComboBox()
                .Name("productComboBox") //The name of the combobox is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
                .Filter(FilterType.Contains)
                .DataSource(source =>
                {
                    source.Read(read =>
                    {
                        read.Action("GetProducts", "Home"); //Set the Action and Controller name
                    })
                    .ServerFiltering(true); //If true the DataSource will not filter the data on the client.
                })
                .SelectedIndex(0) //Select first item.
            %>
    - Razor

            @(Html.Kendo().ComboBox()
                .Name("productComboBox") //The name of the combobox is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
                .Filter(FilterType.Contains)
                .DataSource(source =>
                {
                   source.Read(read =>
                   {
                        read.Action("GetProducts", "Home"); //Set the Action and Controller name
                   })
                   .ServerFiltering(true); //If true the DataSource will not filter the data on the client.
                })
                .SelectedIndex(0) //Select first item.
            )

> **Important**
> The `ToDataSourceResult()` extension method will modify structure of the result and the widget will not be able to bind to it. Please return simple array of data in this case.

### Configure ComboBox to Work with the `ToDataSourceResult` Instance

Here is how to configure the combobox to use a custom datasource and thus to bind to a ToDataSourceResult instance.

1.  Make sure you have followed all the steps from the [Introduction](/aspnet-mvc/introduction) help topic.

2.  Create an action method which renders the view:

        public ActionResult Index()
        {
            return View();
        }
3.  Create a new action method and pass the Products table as Json result:

        public JsonResult GetProducts([DataSourceRequest] DataSourceRequest request)
        {
            NorthwindDataContext northwind = new NorthwindDataContext();

            return Json(northwind.Products.ToDataSourceResult(request));
        }
4.  Add a ajax bound combobox:
    - WebForms

            <%: Html.Kendo().ComboBox()
                .Name("productComboBox")
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
                .DataSource(source =>
                {
                    source.Custom()
                          .ServerFiltering(true)
                          .Type("aspnetmvc-ajax") //Set this type if you want to use DataSourceRequest and ToDataSourceResult instances
                          .Transport(transport =>
                          {
                              transport.Read("GetProducts", "Home");
                          })
                          .Schema(schema =>
                          {
                              schema.Data("Data") //define the [data](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-schema.data) option
                                    .Total("Total"); //define the [total](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-schema.total) option
                          });
                })
            %>
    - Razor

            @(Html.Kendo().ComboBox()
                .Name("productComboBox")
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
                .DataSource(source =>
                {
                    source.Custom()
                          .ServerFiltering(true)
                          .Type("aspnetmvc-ajax") //Set this type if you want to use DataSourceRequest and ToDataSourceResult instances
                          .Transport(transport =>
                          {
                              transport.Read("GetProducts", "Home");
                          })
                          .Schema(schema =>
                          {
                              schema.Data("Data") //define the [data](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-schema.data) option
                                    .Total("Total"); //define the [total](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-schema.total) option
                          });
                })
            )

### Send Parameters to the Server

Here is how to configure the Kendo ComboBox to send parameters to the server:

- WebForms

        <%: Html.Kendo().ComboBox()
                .Name("productComboBox") //The name of the combobox is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
                .Filter(FilterType.Contains)
                .DataSource(source =>
                {
                        source.Read(read =>
                       {
                                read.Action("GetProducts", "Home")
                                    .Data("onAdditionalData");
                       });
                })
                .SelectedIndex(0) //Select first item.
         %>
         <script>
            function onAdditionalData() {
                return {
                    text: $("#productComboBox").data("kendoComboBox").text()
                };
            }
        </script>

- Razor

        @(Html.Kendo().ComboBox()
              .Name("productComboBox") //The name of the combobox is mandatory. It specifies the "id" attribute of the widget.
              .DataTextField("ProductName") //Specifies which property of the Product to be used by the combobox as a text.
              .DataValueField("ProductID") //Specifies which property of the Product to be used by the combobox as a value.
              .Filter(FilterType.Contains)
              .DataSource(source =>
              {
                 source.Read(read =>
                 {
                      read.Action("GetProducts", "Home") //Set the Action and Controller name
                          .Data("onAdditionalData");
                 });
              })
              .SelectedIndex(0) //Select first item.
        )

        <script>
            function onAdditionalData() {
                return {
                    text: $("#productComboBox").data("kendoComboBox").text()
                };
            }
        </script>

Here is how the **GetProducts** method looks like:

        public JsonResult GetProducts(string text)
        {
            var northwind = new SampleEntities();

            var products = northwind.Products.Select(product => new ProductViewModel
                    {
                    ProductID = product.ProductID,
                    ProductName = product.ProductName,
                    UnitPrice = product.UnitPrice ?? 0,
                    UnitsInStock = product.UnitsInStock ?? 0,
                    UnitsOnOrder = product.UnitsOnOrder ?? 0,
                    Discontinued = product.Discontinued
                    });

            if (!string.IsNullOrEmpty(text))
            {
                products = products.Where(p => p.ProductName.Contains(text));
            }

            return Json(products, JsonRequestBehavior.AllowGet);
        }

> **Important**
> The Kendo ComboBox has default event handler for the DataSource's Data callback. If you do not
define event handler, it will be used.

*Default event handler for the DataSource's Data callback*

    function requestData(selector) {
        var combobox = $(selector).data("kendoComboBox"),
            filters = combobox.dataSource.filter(),
            value = combobox.input.val();

        if (!filter || !filter.filters.length) {
            value = "";
        }

        return { text: value };
    }

As you can see the combobox sends the input's value only if the end-user starts to type in it.

### Grouping

The widget supports binding to a grouped data source. The only requirement is to define a datasource group expression in order to group the data.
This can be done using a [Custom DataSource configuration](/aspnet-mvc/custom-datasource). Refer to our online demo for more details:

- [Grouping demo](http://demos.telerik.com/aspnet-mvc/combobox/grouping)

> The data source will sort the grouped data either ascending or descending. If you would like to persist a specific group order, then you will need to use
a [server grouping](/api/javascript/data/datasource#configuration-serverGrouping). Use the DataSource ServerGrouping method to define the `serverGrouping` option.

## Access an Existing ComboBox

You can reference an existing ComboBox instance via [jQuery.data()](http://api.jquery.com/jQuery.data/).
Once a reference has been established, you can use the [API](/api/javascript/ui/combobox#methods) to control its behavior.


### Access an Existing ComboBox Instance

    //Put this after your Kendo ComboBox  for ASP.NET MVC declaration
    <script>
    $(function() {
    // Notice that the Name() of the combobox is used to get its client-side instance
    var combobox = $("#productComboBox").data("kendoComboBox");
    });
    </script>


## Handle ComboBox Events

You can subscribe to all [events](/api/javascript/ui/combobox#events) exposed by Kendo UI ComboBox:

### WebForms - Subscribe by Handler Name

    <%: Html.Kendo().ComboBox()
        .Name("combobox")
        .BindTo(new string[] { "Item1", "Item2", "Item3" })
        .Events(e => e
            .Select("combobox_select")
            .Change("combobox_change")
        )
    %>
    <script>
    function combobox_select() {
        //Handle the select event
    }

    function combobox_change() {
        //Handle the change event
    }
    </script>


### Razor - Subscribe by Handler Name

    @(Html.Kendo().ComboBox()
      .Name("combobox")
      .BindTo(new string[] { "Item1", "Item2", "Item3" })
      .Events(e => e
            .Select("combobox_select")
            .Change("combobox_change")
      )
    )
    <script>
    function combobox_select() {
        //Handle the select event
    }

    function combobox_change() {
        //Handle the change event
    }
    </script>


### Razor - Subscribe by Template Delegate

    @(Html.Kendo().ComboBox()
      .Name("combobox")
      .BindTo(new string[] { "Item1", "Item2", "Item3" })
      .Events(e => e
          .Select(@<text>
            function() {
                //Handle the select event inline
            }
          </text>)
          .Change(@<text>
            function() {
                //Handle the change event inline
            }
            </text>)
      )
    )

