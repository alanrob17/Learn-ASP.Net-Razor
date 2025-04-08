# Adding pagination to ASP.Net Razor Pages

Create a new Model class named ``PaginatedList.cs``.

```bash
public class PaginatedList<T> : List<T>
{
    public int PageIndex { get; private set; }
    public int TotalPages { get; private set; }

    public PaginatedList(List<T> items, int count, int pageIndex, int pageSize)
    {
        PageIndex = pageIndex;
        TotalPages = (int)Math.Ceiling(count / (double)pageSize);

        this.AddRange(items);
    }

    public bool HasPreviousPage => PageIndex > 1;

    public bool HasNextPage => PageIndex < TotalPages;

    public static async Task<PaginatedList<T>> CreateAsync(
        IQueryable<T> source, int pageIndex, int pageSize)
    {
        var count = await source.CountAsync();
        var items = await source.Skip(
            (pageIndex - 1) * pageSize)
            .Take(pageSize)
            .ToListAsync();
        return new PaginatedList<T>(items, count, pageIndex, pageSize);
    }
}
```

Add the following code to the bottom of the ``Movies/Index.cshtml`` page after the ``Table`` code.

```bash
@{
    var prevDisabled = !Model.Movie.HasPreviousPage ? "disabled" : "";
    var nextDisabled = !Model.Movie.HasNextPage ? "disabled" : "";
}

<div class="pagination-container">
    <a asp-page="./Index"
       asp-route-pageIndex="@(Model.Movie.PageIndex - 1)"
       class="btn btn-primary @prevDisabled">
        Previous
    </a>
    <span class="page-number">
        Page @Model.Movie.PageIndex of @Model.Movie.TotalPages
    </span>
    <a asp-page="./Index"
       asp-route-pageIndex="@(Model.Movie.PageIndex + 1)"
       class="btn btn-primary @nextDisabled">
        Next
    </a>
</div>

<style>
    .pagination-container {
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .page-number {
        margin: 0 10px;
    }
</style>
```

Change the ``Movies/Index.cshtml.cs page`` code.

```bash
public class IndexModel : PageModel
{
    private readonly RazorPagesMovie.Data.RazorPagesMovieContext _context;

    public IndexModel(RazorPagesMovie.Data.RazorPagesMovieContext context)
    {
        _context = context;
    }

    public PaginatedList<Movie> Movie { get; set; } = default!;

    [BindProperty(SupportsGet = true)]
    public int CurrentPage { get; set; } = 1;
    public int Count { get; set; }
    public int PageSize { get; set; } = 5;
    public int TotalPages => (int)Math.Ceiling(decimal.Divide(Count, PageSize));

    [BindProperty(SupportsGet = true)]
    public string? SearchString { get; set; }

    public SelectList? Genres { get; set; }

    [BindProperty(SupportsGet = true)]
    public string? MovieGenre { get; set; }

    public async Task OnGetAsync(int? pageIndex)
    {
        IQueryable<string> genreQuery = from m in _context.Movie
                                        orderby m.Genre
                                        select m.Genre;

        var movies = from m in _context.Movie
                     select m;

        if (!string.IsNullOrEmpty(SearchString))
        {
            PageSize = 1000;
            movies = movies.Where(s => s.Title.Contains(SearchString));
        }

        if (!string.IsNullOrEmpty(MovieGenre))
        {
            PageSize = 1000;
            movies = movies.Where(x => x.Genre == MovieGenre);
        }

        CurrentPage = pageIndex ?? 1;

        Genres = new SelectList(await genreQuery.Distinct().ToListAsync());

        // Create paginated list AFTER all filtering is applied
        Count = await movies.CountAsync();
        Movie = await PaginatedList<Movie>.CreateAsync(movies, CurrentPage, PageSize);
    }
}
```

Run the code and it should return.

![Pagination](assets/images/paginated-page.jpg "Pagination")

**Note:** if you add a ``searchString`` or ``Genre`` I have reset the ``PageSize`` to 1000 to remove the pagination. I did this because I can't get the pagination and ``Genre`` or ``searchString`` values to work properly.

![Remove pagination](assets/images/pagination-removed.jpg "Remove pagination")
