# From Controller to View

## What is Controller

- A **Controller** is a **Class**, which inherits _Microsoft.AspNetCore.Mvc_.Controller.
- These classes have a suffix "Controller", like StudentController.
- Controllers have a set of public methods, which are called **Operation Methods**. We use them to process HTTP requests.
- Url will be parsed and point to a certain method of a certain controller.

    _e.g._ http://domainName/home/details will be pointed to **HomeController**'s **Details** method.

    _Router_ and _routing rules_ will parse this link.

- Controllers will create Model to query data. _Services_ will be injected by _Dependency Injection_ into Controllers.
- Usually we make dependencies (services, instances of some interfaces, like _studentRepository) **readonly**. 

<hr>

## Respond Certain Format of Data from Controller

We can force the method in controller return a certain format of data, like JSON, despite the request headers: "Accept Header".

The following code will only response JSON data with headers:
> Content-Type:application/json;

>charset=utf-8
The request header "Accept:application/xml" will be ignored.

```c#
public JsonResult Details()
{
    Student model = _studentRepository.GetStudent(1);
    return Json(model);
}
```

If we want the response format and header to follow the request header, we could use **ObjectResult** as the type of the method's return value:
```c#
public ObjectResult Details()
{
    Student model = _studentRepository.GetStudent(1);
    // P.S. ObjectResult is a Class, but Json is a function
    return new ObjectResult(model);
}
```
For our application could serialize the object correctly, we need to add the corresponding serializer to services.
```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews( a => a.EnableEndpointRouting = false).AddXmlSerializerFormatters();
}
```
Now the response's "Content-Type" header will be either "application/xml" or "application/json" according to the request's "Accept" header.

## Respond View from Controller

To return a View, we need to set the type of the return value as "ViewResult":
```c#
// In a controller
public ViewResult Details()
{
    Student model = _studentRepository.GetStudent(1);
    return View(model);
}
```
If we run without creating corresponding view file, we will get an error:
> InvalidOperationException ：The view 'Details' was not found. The following locations were searched ：/Views/Home/Details.cshtml /Views/Shared/Details.cshtml

Notice View files will be searched by default in the following two paths:
- /Views/Home/Details.cshtml
- /Views/Shared/Details.cshtml

View files are **Razor View** type.

### View() Method

- View() will search cshtml files in the two default paths.
- View(string viewName) will use cshtml file in path viewName. Notice, **viewName** is a **path string**. It could be relative or absolute like "../MyViews/Test", "MyViews/Test.cshtml", "/MyViews/Test.cshtml" or "~/MyViews/Test.cshtml". _Absolute path_ must be with ".cshtml" suffix, but _Relative Path_ could ignore the ".cshtml" at the end.
- View(object model) will transfer the object model into the default view file.
- View(string viewName, object model) will choose view file viewName and pass the model into it.

## Transferring Data into View

Three methods could be used to transfer data:
- ViewData
- ViewBag
- Strong type model object, also called _Strong-Type View_

### Strong Type Model

This is the most common method to transfer data into view. It supports error checking pre-compiling.

Using **@model** directive to declare the type of model passed into view. When we try to access the properties, we use **@Model** (notice the capital M).

```c#
public ViewResult Details()
{
    Student model = _studentRepository.GetStudent(1);
    ViewBag.PageTile = "Student Details";

    return View(model);
}
```

Details.cshtml with Razor symbols inside (@ directive). Notice the first line is the declaration for model.
```cshtml
@model MockSchoolManage.Models.Student
<html>
    <head>
        <title></title>
    </head>
    <body>
        <h3>@ViewBag.PageTitle</h3>
        <div>
            Name: @Model.Name
        </div>
        <div>
            Email: @Model.Email
        </div>
        <div>
            Major: @Model.Major
        </div>
    </body>
</html>
```

### ViewData

ViewData is a weak type **Dictionary Type Object**, which could be indexed by string type keyword.

It doesn't support pre-compiling error checking.

```c#
public ViewResult Details ()
{
    Student model = _studentRepository.GetStudent(1);
    ViewData["PageTitle"] = "Student Details";
    ViewData["Student"] = model;

    return View();
}
```

Details.cshtml is a html page with Razor symbols inside (@ directive).
```cshtml
@using MockSchoolManage.Models
<html>
    <head>
        <title></title>
    </head>
    <body>
        <h3>@ViewData["PageTitle"]</h3>
        @{ var student = ViewData["Student"] as Student; }
        <div>
            Name: @student.Name
        </div>
        <div>
            Email: @student.Email
        </div>
        <div>
            Major: @student.Major
        </div>
    </body>
</html>
```

### ViewBag

ViewBag is a decorator for ViewData. The only difference is ViewData uses string keyword, but ViewBag uses Dynamic properties.


```c#
public ViewResult Details ()
{
    Student model = _studentRepository.GetStudent(1);
    ViewBag.PageTitle = "Student Details";
    ViewDataBag.Student = model;

    return View();
}
```

Details.cshtml is a html page with Razor symbols inside (@ directive).
```cshtml
<html>
    <head>
        <title></title>
    </head>
    <body>
        <h3>@ViewDataBag.PageTitle</h3>
        <div>
            Name: @ViewBag.student.Name
        </div>
        <div>
            Email: @ViewBag.student.Email
        </div>
        <div>
            Major: @ViewBag.student.Major
        </div>
    </body>
</html>
```