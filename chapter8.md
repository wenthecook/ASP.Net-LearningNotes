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