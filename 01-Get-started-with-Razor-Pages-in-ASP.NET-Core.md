# Get started with Razor Pages in ASP.NET Core

[Microsoft tutorial](https://learn.microsoft.com/en-us/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-9.0&tabs=visual-studio).

At the end of this tutorial, you'll have a Razor Pages web app that manages a database of movies.

## Examine the project files

The following sections contain an overview of the main project folders and files that you'll work with in later tutorials.

## Pages folder

Contains Razor pages and supporting files. Each Razor page is a pair of files:

* A .cshtml file that has HTML markup with C# code using Razor syntax.
* A .cshtml.cs file that has C# code that handles page events.

Supporting files have names that begin with an underscore. For example, the ``_Layout.cshtml`` file configures UI elements common to all pages. ``_Layout.cshtml`` sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page. For more information, see [Layout in ASP.Net Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/layout?view=aspnetcore-9.0).

### wwwroot folder

Contains static assets, like HTML files, JavaScript files, and CSS files. For more information, see [Static files in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-9.0).

### appsettings.json

Contains configuration data, like connection strings. For more information, see [Configuration in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-9.0).

### Program.cs

Contains the following code:

```bash
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
    app.UseHsts();
}

app.UseHttpsRedirection();

app.UseRouting();

app.UseAuthorization();

app.MapStaticAssets();
app.MapRazorPages();

app.Run();
```

The following lines of code in this file create a WebApplicationBuilder with preconfigured defaults, add Razor Pages support to [the Dependency Injection (DI) container](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-9.0), and builds the app:

```bash
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();

var app = builder.Build();
```

The developer exception page is enabled by default and provides helpful information on exceptions. Production apps should not be run in development mode because the developer exception page can leak sensitive information.

The following code sets the exception endpoint to /Error and enables [HTTP Strict Transport Security Protocol (HSTS)](https://learn.microsoft.com/en-us/aspnet/core/security/enforcing-ssl?view=aspnetcore-9.0#http-strict-transport-security-protocol-hsts) when the app is not running in development mode:

```bash
// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
    app.UseHsts();
}
```

For example, the preceding code runs when the app is in production or test mode. For more information, see [Use multiple environments in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-9.0).

The following code enables various [Middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/?view=aspnetcore-9.0):

``app.UseHttpsRedirection();`` : Redirects HTTP requests to HTTPS.

``app.UseRouting();`` : Adds route matching to the middleware pipeline. For more information, see [Routing in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/routing?view=aspnetcore-9.0).

``app.UseAuthorization();`` : Authorizes a user to access secure resources. This app doesn't use authorization, therefore this line could be removed.

``app.MapRazorPages();`` : Configures endpoint routing for Razor Pages.

``app.MapStaticAssets();`` : Optimize the delivery of static assets in an app, such as HTML, CSS, images, and JavaScript. 

``app.Run();`` : Runs the app.
