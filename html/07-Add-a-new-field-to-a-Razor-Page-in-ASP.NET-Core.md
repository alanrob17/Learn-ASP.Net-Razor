# Add a new field to a Razor Page in ASP.NET Core

In this section [Entity Framework Core (EF Core)](https://learn.microsoft.com/en-us/ef/core/get-started/aspnetcore/new-db) is used to define the database schema based on the app's model class:

* Add a new field to the model.
* Migrate the new field schema change to the database.

The EF Core approach allows for a more agile development process. The developer works on the app's data model directly while the database schema is created and then synchronized, all without the developer having to switch contexts to and from a database management tool. For an overview of Entity Framework Core and its benefits, see [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core).

Using EF Code to automatically create and track a database:

Adds an [__EFMigrationsHistory](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/history-table) table to the database to track whether the schema of the database is in sync with the model classes it was generated from.
Throws an exception if the model classes aren't in sync with the database.

Automatic verification that the schema and model are in sync makes it easier to find inconsistent database code issues.

## Adding a Rating Property to the Movie Model

Open the ``Models/Movie.cs`` file and add a Rating property:

```bash
    public string Rating { get; set; } = string.Empty;
```

Edit ``Pages/Movies/Index.cshtml``, and add a Rating field:

```bash
@page
@model RazorPagesMovie.Pages.Movies.IndexModel

@{
    ViewData["Title"] = "Index";
}

<h1>Index</h1>

<p>
    <a asp-page="Create">Create New</a>
</p>

<form>
    <p>
        <select asp-for="MovieGenre" asp-items="Model.Genres">
            <option value="">All</option>
        </select>
        <label>Title: <input type="text" asp-for="SearchString" /></label>
        <input type="submit" value="Filter" />
    </p>
</form>

<table class="table">

    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Movie[0].Title)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movie[0].ReleaseDate)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movie[0].Genre)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movie[0].Price)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movie[0].Rating)
            </th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Model.Movie)
        {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Title)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.ReleaseDate)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Genre)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Price)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Rating)
                </td>
                <td>
                    <a asp-page="./Edit" asp-route-id="@item.Id">Edit</a> |
                    <a asp-page="./Details" asp-route-id="@item.Id">Details</a> |
                    <a asp-page="./Delete" asp-route-id="@item.Id">Delete</a>
                </td>
            </tr>
        }
    </tbody>
</table>
```

Update the following pages with a ``Rating`` field:

### Pages/Movies/Create.cshtml

```bash
﻿@page
@model RazorPagesMovie.Pages.Movies.CreateModel

@{
    ViewData["Title"] = "Create";
}

<h1>Create</h1>

<h4>Movie</h4>
<hr />
<div class="row">
    <div class="col-md-4">
        <form method="post">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <div class="form-group">
                <label asp-for="Movie.Title" class="control-label"></label>
                <input asp-for="Movie.Title" class="form-control" />
                <span asp-validation-for="Movie.Title" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.ReleaseDate" class="control-label"></label>
                <input asp-for="Movie.ReleaseDate" class="form-control" />
                <span asp-validation-for="Movie.ReleaseDate" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.Genre" class="control-label"></label>
                <input asp-for="Movie.Genre" class="form-control" />
                <span asp-validation-for="Movie.Genre" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.Price" class="control-label"></label>
                <input asp-for="Movie.Price" class="form-control" />
                <span asp-validation-for="Movie.Price" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.Rating" class="control-label"></label>
                <input asp-for="Movie.Rating" class="form-control" />
                <span asp-validation-for="Movie.Rating" class="text-danger"></span>
            </div>
            <div class="form-group">
                <input type="submit" value="Create" class="btn btn-primary" />
            </div>
        </form>
    </div>
</div>

<div>
    <a asp-page="Index">Back to List</a>
</div>

@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
}
```

### Pages/Movies/Delete.cshtml

```bash
@page
@model RazorPagesMovie.Pages.Movies.DeleteModel

@{
    ViewData["Title"] = "Delete";
}

<h1>Delete</h1>

<h3>Are you sure you want to delete this?</h3>
<div>
    <h4>Movie</h4>
    <hr />
    <dl class="row">
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Title)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Title)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.ReleaseDate)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.ReleaseDate)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Genre)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Genre)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Price)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Price)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Rating)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Rating)
        </dd>
    </dl>
    
    <form method="post">
        <input type="hidden" asp-for="Movie.Id" />
        <input type="submit" value="Delete" class="btn btn-danger" /> |
        <a asp-page="./Index">Back to List</a>
    </form>
</div>
```

### Pages/Movies/Details.cshtml

```bash
﻿@page
@model RazorPagesMovie.Pages.Movies.DetailsModel

@{
    ViewData["Title"] = "Details";
}

<h1>Details</h1>

<div>
    <h4>Movie</h4>
    <hr />
    <dl class="row">
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Title)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Title)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.ReleaseDate)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.ReleaseDate)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Genre)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Genre)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Movie.Price)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Price)
        </dd>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Movie.Rating)
        </dd>
    </dl>
</div>
<div>
    <a asp-page="./Edit" asp-route-id="@Model.Movie.Id">Edit</a> |
    <a asp-page="./Index">Back to List</a>
</div>
```

### Pages/Movies/Edit.cshtml

```bash
﻿@page
@model RazorPagesMovie.Pages.Movies.EditModel

@{
    ViewData["Title"] = "Edit";
}

<h1>Edit</h1>

<h4>Movie</h4>
<hr />
<div class="row">
    <div class="col-md-4">
        <form method="post">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <input type="hidden" asp-for="Movie.Id" />
            <div class="form-group">
                <label asp-for="Movie.Title" class="control-label"></label>
                <input asp-for="Movie.Title" class="form-control" />
                <span asp-validation-for="Movie.Title" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.ReleaseDate" class="control-label"></label>
                <input asp-for="Movie.ReleaseDate" class="form-control" />
                <span asp-validation-for="Movie.ReleaseDate" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.Genre" class="control-label"></label>
                <input asp-for="Movie.Genre" class="form-control" />
                <span asp-validation-for="Movie.Genre" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.Price" class="control-label"></label>
                <input asp-for="Movie.Price" class="form-control" />
                <span asp-validation-for="Movie.Price" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Movie.Rating" class="control-label"></label>
                <input asp-for="Movie.Rating" class="form-control" />
                <span asp-validation-for="Movie.Rating" class="text-danger"></span>
            </div>
            <div class="form-group">
                <input type="submit" value="Save" class="btn btn-primary" />
            </div>
        </form>
    </div>
</div>

<div>
    <a asp-page="./Index">Back to List</a>
</div>

@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
}
```

The app won't work until the database is updated to include the new field. Running the app without an update to the database throws a ``SqlException``:

``SqlException: Invalid column name 'Rating'``.

The ``SqlException`` exception is caused by the updated Movie model class being different than the schema of the Movie table of the database. There's no ``Rating`` column in the database table.

There are a few approaches to resolving the error:

1. Have the Entity Framework automatically drop and re-create the database using the new model class schema. This approach is convenient early in the development cycle, it allows developers to quickly evolve the model and database schema together. The downside is that existing data in the database is lost. Don't use this approach on a production database! Dropping the database on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.
2. Explicitly modify the schema of the existing database so that it matches the model classes. The advantage of this approach is to keep the data. Make this change either manually or by creating a database change script.
Use EF Core Migrations to update the database schema.
For this tutorial, use EF Core Migrations.
3. Update the SeedData class so that it provides a value for the new column. A sample change is shown below, but make this change for each new Movie block.

For this tutorial, use EF Core Migrations.

Update the ``SeedData`` class so that it provides a value for the new column. A sample change is shown below, but make this change for each ``new Movie`` block.

```bash
    context.Movie.AddRange(
        new Movie
        {
            Title = "When Harry Met Sally",
            ReleaseDate = DateTime.Parse("1989-2-12"),
            Genre = "Romantic Comedy",
            Price = 7.99M,
            Rating = "R"
        },
```

Now build the application.

## Add a migration for the rating field

From the Tools menu, select ``NuGet Package Manager > Package Manager Console``.

In the Package Manager Console (PMC), enter the following command:

```bash
    Add-Migration Rating
```

The Add-Migration command tells the framework to:

Compare the ``Movie`` model with the ``Movie`` database schema.

Create code to migrate the database schema to the new model.
The name "Rating" is arbitrary and is used to name the migration file. It's helpful to use a meaningful name for the migration file.

In the PMC, enter the following command:

```bash
    Update-Database
```

The ``Update-Database`` command tells the framework to apply the schema changes to the database and to preserve existing data.

**Delete** all the records in the database, the initializer will seed the database and include the Rating field. Deleting can be done with the delete links in the browser or from [Sql Server Object Explorer (SSOX)](https://learn.microsoft.com/en-us/aspnet/core/tutorials/razor-pages/sql?view=aspnetcore-9.0#ssox).

### Another option 

Is to delete the database and use migrations to re-create the database. To delete the database in SSOX:

Select the database in SSOX.

Right-click on the database, and select Delete.

Check Close existing connections.

Select OK.

In the PMC, update the database:

```bash
    Update-Database
```

Run the app and verify you can create, edit, and display movies with a ``Rating`` field. If the database isn't seeded, set a break point in the ``SeedData.Initialize`` method.