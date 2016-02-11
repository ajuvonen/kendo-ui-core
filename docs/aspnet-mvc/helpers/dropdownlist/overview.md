---
title: Overview
page_title: DropDownList HtmlHelper extension for Kendo UI DropDownList | Kendo UI documentation
description: How to bind a Kendo UI DropDownList for ASP.NET MVC, configure a server-side wrapper for Kendo UI DropDownlist Widget.
---

# DropDownList

The DropDownList HtmlHelper extension is a server-side wrapper for the [Kendo UI DropDownList](/api/javascript/ui/dropdownlist) widget.

## Getting Started

There are two ways to bind a Kendo DropDownList for ASP.NET MVC:

*   **server** - the data will be serialized to the client. No Ajax requests will be made.
*   **ajax** - the dropdownlist will make ajax request to get the data.

### Configure the DropDownList for Server Binding

Here is how to configure the Kendo DropDownList  for server binding to the Northwind Products table using Linq to SQL:

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
4.  Add a server bound dropdownlist:
    - WebForms

            <%: Html.Kendo().DropDownList()
                .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
                .BindTo(Model)   //Pass the list of Products to the dropdownlist.
                .SelectedIndex(10) //Select an item with index 10\. Note that the indexes are zero based.
            %>
    - Razor

            @(Html.Kendo().DropDownList()
              .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
              .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
              .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
              .BindTo(Model)   //Pass the list of Products to the dropdownlist.
              .SelectedIndex(10) //Select an item with index 10\. Note that the indexes are zero based.
            )

### Configure the DropDownList for Ajax Binding

Here is how to configure the Kendo DropDownList for ajax binding to the Northwind Products table using Linq to SQL:

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
4.  Add a ajax bound dropdownlist:
    - WebForms

            <%: Html.Kendo().DropDownList()
                .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
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

            @(Html.Kendo().DropDownList()
                .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
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
> The `ToDataSourceResult()` extension method will modify the structure of the result and the widget will not be able to bind to it. Please return simple array of data in this case.

### Configure the DropDownList to Work with the `ToDataSourceResult` Instance

Here is how to configure the dropdownlist to use a custom datasource and thus to bind to a ToDataSourceResult instance.

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
4.  Add a ajax bound dropdownlist:
    - WebForms

            <%: Html.Kendo().DropDownList()
                .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
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

            @(Html.Kendo().DropDownList()
                .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
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

Here is how to configure the Kendo DropDownList to send parameters to the server:

- WebForms

        <%: Html.Kendo().DropDownList()
                .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
                .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
                .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
                .Filter(FilterType.Contains)
                .DataSource(source =>
                {
                        source.Read(read =>
                       {
                                read.Action("GetProducts", "Home")
                                    .Data("onAdditionalData");
                       });
                })
         %>
         <script>
            function onAdditionalData() {
                return {
                    text: $("#productDropDownList").data("kendoDropDownList").text()
                };
            }
        </script>

- Razor

        @(Html.Kendo().DropDownList()
              .Name("productDropDownList") //The name of the dropdownlist is mandatory. It specifies the "id" attribute of the widget.
              .DataTextField("ProductName") //Specifies which property of the Product to be used by the dropdownlist as a text.
              .DataValueField("ProductID") //Specifies which property of the Product to be used by the dropdownlist as a value.
              .Filter(FilterType.Contains)
              .DataSource(source =>
              {
                 source.Read(read =>
                 {
                      read.Action("GetProducts", "Home") //Set the Action and Controller name
                          .Data("onAdditionalData");
                 });
              })
        )

        <script>
            function onAdditionalData() {
                return {
                    text: $("#productDropDownList").data("kendoDropDownList").filterInput.val()
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
> The DropDownList has default event handler for the DataSource's Data callback. If you do not define an event handler, the default one will be used.

*Default event handler for the DataSource's Data callback*

    function requestData(selector) {
        var dropdownlist = $(selector).data("kendoDropDownList"),
            filters = dropdownlist.dataSource.filter(),
            value = dropdownlist.filterInput.val();

        if (!filter || !filter.filters.length) {
            value = "";
        }

        return { text: value };
    }

As you can see the dropdownlist sends the input's value only if the end-user starts to type in it.

### Grouping

The widget supports binding to a grouped data source. The only requirement is to define a datasource group expression in order to group the data.
This can be done using a [Custom DataSource configuration](/aspnet-mvc/custom-datasource). Refer to our online demo for more details:

- [Grouping demo](http://demos.telerik.com/aspnet-mvc/dropdownlist/grouping)

> The data source will sort the grouped data either ascending or descending. If you would like to persist a specific group order, then you will need to use
a [server grouping](/api/javascript/data/datasource#configuration-serverGrouping). Use the DataSource ServerGrouping method to define the `serverGrouping` option.

## Access an Existing DropDownList

You can reference an existing DropDownList instance via [jQuery.data()](http://api.jquery.com/jQuery.data/).
Once a reference has been established, you can use the [API](/api/javascript/ui/dropdownlist#methods) to control its behavior.


### Access an Existing DropDownList Instance

    //Put this after your Kendo DropDownList for ASP.NET MVC declaration
    <script>
    $(function() {
    // Notice that the Name() of the dropdownlist is used to get its client-side instance
    var dropdownlist = $("#productDropDownList").data("kendoDropDownList");
    });
    </script>


## Handle DropDownList Events

You can subscribe to all [events](/api/javascript/ui/dropdownlist#events) exposed by Kendo UI DropDownList:


### WebForms - Subscribe by Handler Name

    <%: Html.Kendo().DropDownList()
        .Name("dropdownlist")
        .BindTo(new string[] { "Item1", "Item2", "Item3" })
        .Events(e => e
            .Select("dropdownlist_select")
            .Change("dropdownlist_change")
        )
    %>
    <script>
    function dropdownlist_select() {
        //Handle the select event
    }

    function dropdownlist_change() {
        //Handle the change event
    }
    </script>


### Razor - Subscribe by Handler Name

    @(Html.Kendo().DropDownList()
      .Name("dropdownlist")
      .BindTo(new string[] { "Item1", "Item2", "Item3" })
      .Events(e => e
            .Select("dropdownlist_select")
            .Change("dropdownlist_change")
      )
    )
    <script>
    function dropdownlist_select() {
        //Handle the select event
    }

    function dropdownlist_change() {
        //Handle the change event
    }
    </script>


### Razor - Subscribe by Template Delegate

    @(Html.Kendo().DropDownList()
      .Name("dropdownlist")
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

