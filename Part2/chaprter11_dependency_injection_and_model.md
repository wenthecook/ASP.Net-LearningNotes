# Dependency Injection and Model

## Set up Student Model

### **1. Create a folder "/Models". Then create a class **Student** inside**

It is not necessary to put models inside this folder, but it is a good practice.

```c#
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    /// <summary>
    /// Major classes
    /// </summary>
    public string Major { get; set; }

    public string Email { get; set; }
}
```

### **2. Create a folder "/DataRepositories"**

Create an _Interface_ __IStudentRepository__.
```c#
public interface IStudentRepository
{
    Student GetStudent(int id);
}
```
Create an _service_ (class) **MockStudentRepository**, which is an implementation of interface **IStudentRepository**.
```c#
public class MockStudentRepository : IStudentRepository
{
    private List<Student> _studentList;

    public MockStudentRepository()
    {
        _studentList =  new List<Student>()
        {
            new Student() 
            { 
                Id = 1, 
                Name = "Bob", 
                Major = "CS", 
                Email = "bob@gmail.com"
            },
            new Student(),
            { 
                Id = 2, 
                Name = "Alice", 
                Major = "Delivery", 
                Email = "alice@gmail.com"
            },
            new Student(),
            { 
                Id = 3, 
                Name = "John", 
                Major = "Electronic Business", 
                Email = "john@gmail.com"
            }
        };
    }

    public Student GetStudent(int id)
    {
        return _studentList.FirstOrDefault( a => a.Id == id );
    }
}
```
In the future we just write code aiming the interface **IStudentRepository**, instead of the class MockStudentRepository. It's more flexible and easy to do unit test. This kind of abstract allows us to use **Dependency Injection**.

### **3. Service injection**

Use constructor to inject the implementation into the current class. In the following case, we make the implementation of _IStudentRepository_ independent from _HomeController_.
```c#
public class HomeController : Controller 
{
    private readonly IStudentRepository _studentRepository;

    public HomeController(IStudentRepository studentRepository)
    {
        _studentRepository = studentRepository;
    }

    public string Index()
    {
        return _studentRepository.GetStudent(1).Name;
    }
}
```

If IStudentRepository is not injected, there will be an error:

> InvalidOperationException ：Unable to resolve **service** for type 'MockSchoolManagement.DataRepositories.IStudentRepository' while attempting to activate 'MockSchoolManagement.Controllers.HomeController'.

To solve this, let's use **Dependency Injection** register the service.

```c#
// In Startup.cs

public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews( a => a.EnableEndpointRouting = false );

    // Use AddSingleton<interface, implementation>() to register a service
    services.AddSingleton<IStudentRepository, MockStudentRepository>();
}
```
There are three methods which could be used to inject service.

> **AddSingleton()** Create a Singleton service. It will create an instance when the first request comes, and will use it the entire lifetime of the web application.

> **AddTransient()** Create an temporary _Transient_ service. It will create a new instance for each request.

> **AddScoped()** Create a _Scoped_ service, which could be used in a specific scope. For example, it will create one and only one instance for each http request. But in different requests the instances are different.
