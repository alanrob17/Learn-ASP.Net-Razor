# Add a model to a Razor Pages app in ASP.NET Core
In this tutorial, classes are added for managing movies in a database. The app's model classes use [Entity Framework Core (EF Core)](https://learn.microsoft.com/en-us/ef/core) to work with the database. EF Core is an object-relational mapper (O/RM) that simplifies data access. You write the model classes first, and EF Core creates the database.

The model classes are known as ``POCO`` classes (from "Plain-Old CLR Objects") because they don't have a dependency on EF Core. They define the properties of the data that are stored in the database.

In Solution Explorer, right-click the ``RazorPagesMovie`` project > Add > New Folder. Name the folder ``Models``.

Right-click the ``Models`` folder. Select Add > Class. Name the class ``Movie``.

Add the following properties to the ``Movie`` class:

```bash
using System.ComponentModel.DataAnnotations;

namespace RazorPagesMovie.Models;

public class Movie
{
    public int Id { get; set; }
    public string? Title { get; set; }
    [DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }
    public string? Genre { get; set; }
    public decimal Price { get; set; }
}
```

The Movie class contains:

The ``ID`` field is required by the database for the primary key.

A [DataType](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.datatypeattribute) attribute that specifies the type of data in the ReleaseDate property. With this attribute:

The user isn't required to enter time information in the date field. 

Only the date is displayed, not time information.

The question mark after string indicates that the property is ``nullable``. For more information, see [Nullable reference types](https://learn.microsoft.com/en-us/dotnet/csharp/nullable-references).

[DataAnnotations](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.

Build the project to verify there are no compilation errors.

## Scaffold the movie model

In this section, the movie model is scaffolded. That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.

Create the ``Pages/Movies`` folder:

Right-click on the Pages folder > Add > New Folder. Name the folder ``Movies``.

Right-click on the ``Pages/Movies`` folder > Add > New Scaffolded Item.

In the Add New Scaffold dialog, select ``Razor Pages using Entity Framework (CRUD)`` > Add.

Complete the Add Razor Pages using Entity Framework (CRUD) dialog:

In the Model class drop down, select ``Movie (RazorPagesMovie.Models)``.

In the Data context class row, select the ``+ (plus)`` sign.

In the Add Data Context dialog, the class name ``RazorPagesMovie.Data.RazorPagesMovieContext`` is generated.

In the Database provider drop down, select ``SQL Server``.

Select ``Add``.

### Files created and updated

The scaffold process creates the following files:

Pages/Movies: Create, Delete, Details, Edit, and Index.

Data/RazorPagesMovieContext.cs

The created files are explained in the next tutorial.

The scaffold process adds the following code to the ``Program.cs`` file:

```bash
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using RazorPagesMovie.Data;
namespace RazorPagesMovie
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);
            builder.Services.AddDbContext<RazorPagesMovieContext>(options =>
                options.UseSqlServer(builder.Configuration.GetConnectionString("RazorPagesMovieContext") ?? throw new InvalidOperationException("Connection string 'RazorPagesMovieContext' not found.")));

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
            app.MapRazorPages()
               .WithStaticAssets();

            app.Run();
        }
    }
}
```

## Create the initial database schema using EF's migration feature

The migrations feature in Entity Framework Core provides a way to:

Create the initial database schema.

Incrementally update the database schema to keep it in sync with the app's data model. Existing data in the database is preserved.

In this section, the **Package Manager Console (PMC)** window is used to:

Add an initial migration.

Update the database with the initial migration.

From the Tools menu, select **NuGet Package Manager > Package Manager Console**.

In the PMC, enter the following command:

```bash
    Add-Migration InitialCreate
```

The ``Add-Migration`` command generates code to create the initial database schema. The schema is based on the model specified in ``DbContext``. The ``InitialCreate`` argument is used to name the migration. Any name can be used, but by convention a name is selected that describes the migration.

The following warning is displayed, which is addressed in a later step:

No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.

In the PMC, enter the following command:

```bash
    Update-Database
```

The ``Update-Database`` command runs the ``Up`` method in migrations that have not been applied. In this case, the command runs the ``Up`` method in the ``Migrations/<time-stamp>_InitialCreate.cs`` file, which creates the database.

The data context ``RazorPagesMovieContext``:

Derives from [Microsoft.EntityFrameworkCore.DbContext](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext).

Specifies which entities are included in the data model.

Coordinates EF Core functionality, such as Create, Read, Update and Delete, for the ``Movie`` model.

The ``RazorPagesMovieContext`` class in the generated file ``Data/RazorPagesMovieContext.cs``:

```bash
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.EntityFrameworkCore;
using RazorPagesMovie.Models;

namespace RazorPagesMovie.Data
{
    public class RazorPagesMovieContext : DbContext
    {
        public RazorPagesMovieContext (DbContextOptions<RazorPagesMovieContext> options)
            : base(options)
        {
        }

        public DbSet<RazorPagesMovie.Models.Movie> Movie { get; set; } = default!;
    }
}
```

The preceding code creates a [DbSet<Movie>](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set. In Entity Framework terminology, an entity set typically corresponds to a database table. An entity corresponds to a row in the table.

The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object. For local development, the [Configuration system](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-9.0) reads the connection string from the ``appsettings.json`` file.

### Test the app

Run the app and append ``/Movies`` to the URL in the browser (http://localhost:port/movies). Add a new movie, edit it and delete it.

## Examine the context registered with dependency injection

ASP.NET Core is built with [dependency injection](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-9.0). Services, such as the EF Core database context, are registered with dependency injection during application startup. Components that require these services (such as Razor Pages) are provided via constructor parameters. The constructor code that gets a database context instance is shown later in the tutorial.

The scaffolding tool automatically created a database context and registered it with the dependency injection container. The following highlighted code is added to the ``Program.cs`` file by the scaffolder:

```bash
builder.Services.AddDbContext<RazorPagesMovieContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("RazorPagesMovieContext") ?? throw new InvalidOperationException("Connection string 'RazorPagesMovieContext' not found.")));
```
