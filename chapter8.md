# Static Files

Static files are put in folder /wwwroot
```c#
app.UseDefaultFiles();
// Index.htm -> Index.html -> default.htm -> default.html

// OR use bellow to change the default response
DefaultFilesOptions defaultFilesOptions = new DefaultFilesOptions();
defaultFilesOptions.DefaultFileNames.Clear();
defaultFilesOptions.DefaultFileNames.Add("customisedDefault.html");
app.UseDefaultFiles(defaultFilesOptions);

app.UseStaticFiles();

//UserFileServer() => Combination of UserStaticFiles(), UseDefaultFiles() and UseDirectoryBrowser(). UseDirectoryBrowser() makes the app support directory browsing.
FileServerOptions fileServerOptions = new FileServerOptions();
fileServerOptions.DefaultFilesOptions.DefaultFileNames.Clear();
fileServerOptions.DefaultFilesOptions.DefaultFileNames.Add("customisedDefault.html");
app.UserFileServer(fileServerOptions);
```

## Commonly used Middleware

**UseDeveloperExceptionPage** with _DeveloperExceptionPageOption**s**_
```c#
DeveloperExceptionPageOptions developerExceptionPageOptions = new DeveloperExceptionPageOptions
{
    SourceCodeLineCount = 3
};
```

**UseDefaultFiles** with _DefaultFilesOption**s**_
```c#
DefaultFilesOptions defaultFilesOptions = new DefaultFilesOptions();
defaultFilesOptions.DefaultFileNames.Clear();
defaultFilesOptions.DefaultFileNames.Add("file.html");
```

**UseStaticFiles** with _StaticFileOption**s**_

**UseFileServer** with _FileServerOption**s**_
```C#
FileServerOptions fileServerOptions = new FileServerOptions();
fileServerOptions.DefaultFilesOptions.DefaultFileNames.Clear();
fileServerOptions.DefaultFilesOptions.DefaultFileNames.add("file.html");
```