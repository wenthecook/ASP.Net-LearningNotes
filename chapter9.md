# Chapter 9 Developer Exception Page

## Environment Variable

### ASPNETCORE_ENVIRONMENT
To get the access to this variable, we should inject the **IWebHostEnvironment** service. The reference is *IWebHostEnvironment.EnvironmentName*

This Variable could be set in system environment variables.

Default value is "Production".

Value in *launchsettings.json* will overwrite the system settings.

```c#
public static Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if(env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    app.Run(async(context) => 
    {
        await context.Response.WriteAsync("Hosting Environment: "
            + env.EnvironmentName);
    });
    ...
}
```

### Customise Developer Exception Page
Manually set how many lines showed before and after the line where error happens:
```c#
if (env.IsDevelopment())
{
    DeveloperExceptionPageOptions developerExceptionPageOptions = new DeveloperExceptionPageOptions
    {
        // Show 3 lines before and after the line where error happens
        SourceCodeLineCount = 3;
    }
    app.UseDeveloperExceptionPage(developerExceptionPageOptions);
}
```

env (IWebHostEnvironment) has methods to detect the current Environment:
- IsDevelopment()
- IsStaging()
- IsProduction() // the default
- IsEnvironment(string Env);
