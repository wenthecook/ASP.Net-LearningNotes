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

1. In ConfigureServices() method of Startup class, add a line
    ```c#
    services.AddMvc( a => a.EnableEndpointRouting = false; )
    ```
    This is for using MVC's default Routing. 
2. In Configure() method, add line
    ```C#
    app.UseMvcWithDefaultRoute();
    ```
    This one should be after UseStaticFiles