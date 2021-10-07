# MVC

## What is MVC
- Controller - Process requests, choose Model, choose View, pass Model to View
- Model - Classes, which have data, have access to database or can process data
- View - Decide how to show the data given by Model, most likely a *html template file*. 

**Steps**
1. Controller chooses Models
2. Models get data from databases and process it.
3. Controller chooses View and *pass Model object to View*
4. View decides how to show the data contains in Model object => generate html pages

**Mapping Requests** "/student/details/1" will be mapped to *StudentController*'s *Details()* method, and 1 will be passed as the parameter to Details() method.

## Set up MVC

1. In **ConfigureServices()** method of Startup class, add a line
    ```c#
    services.AddMvc( a => a.EnableEndpointRouting = false; )
    ```
    [comment]: <> (TODO: Fixing link)
    This is for using MVC's default Routing. [MVC Routing configuration](https://link)
2. In Configure() method, add line
    ```C#
    app.UseMvcWithDefaultRoute();
    ```
    This one should be after UseStaticFiles(), to let static files middleware make a short circuit to avoid the unnecessary routing.

    [comment]: <> (TODO: Fixing link)
    There is another middleware called "UseMVC()", we will discuss in the future. [UseMvc()](https://link)

    If now we request a link "http//localhost:XXXX", since it is not an static file request, it will be passed to MVC middleware. We don't specify the **controller**'s name and method, it will be parsed as **HomeController**'s **Index()** method. **HomeController.cs** contains the HomeController class and should be put inside /Controllers folder.
    
    If the method requested is missing, the request will be passed to next middleware. Because **Index()** method is missing, the request will be passed to next middleware. If **UseMvcWithDefaultRoute()** is the last one, the response will be "404 not found".

3. Add Controllers

    Build a new folder **Controllers**, and add a new class named **HomeController**.
    ```c#
    public class HomeController
    {
        public string Index()
        {
            return "Hello from MVC";
        }
    }
    ```
    Notice the type of Index()'s return value. It's now a string. In the future we are gonna return a View.

## Analyse AddMvc() and AddMvcCore()

From .NET Core 3.0, AddMvc() changed a lot. In 2.x, AddMvc() will directly introduce AddMvcCore() and at that time it doesn't support JsonResult.

According to the source code of .net core 3.0, if we do not need Razor pages, we can simply change the code to
```c#
// services.AddMvc( a => { a.EnableEndpointRouting = false });
service.AddControllerWithViews( a => { a.EnableEndpointRouting = false; });
```
