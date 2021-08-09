# Book List Application

>
>
## Desciption

It archives book properties including name, author, ISBN in MS SQL by operating as CRUD and shows Data table in browser.

>
>

## Technology stacks

**.NET Core(C#)**, **Razor Page**, **Tag Helper**, **JavaScript**,**Bootstrap**, **REST API**, **Datatabl API**, **Sweetalert API**, and **MS SQL** 

>


### Database Context

```json
"ConnectionStrings": {
    "DefaultConnection": "Server=(LocalDB)\\MSSQLLocalDB; Database=BookListRazor; Trusted_Connection=True; MultipleActiveResultSets=True"
  }
 ```
 ```cs
      public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContext<ApplicationDbContext>(option => option.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
            services.AddControllersWithViews();
            services.AddRazorPages().AddRazorRuntimeCompilation();
        }
```
>
>
>
### Datatable API

```js
let dataTable;

$(document).ready(function () {
    loadDataTable();
})

function loadDataTable() {
    dataTable = $('#DT_load').DataTable({
        "ajax": {
            "url": "api/book",
            "type": "GET",
            "datatype": "json"
        },
        "columns": [
            { "data": "name", "width": "20%" },
            { "data": "author", "width": "20%" },
            { "data": "isbn", "width": "20%" },
            {
                "data": "id",
                "render": function (data) {
                    return `
                          <div class="text-center">
                           <a href="/BookList/Edit?id=${data}" class="btn btn-success text-white" style="cursor:pointer; width:70px;">
                            Edit
                           </a>
                           &nbsp;
                           <a class="btn btn-danger text-white" style="cursor:pointer; width:70px;"
                             onClick= Delete('api/book?id='+${data})>
                           Delete
                           </a>
                          </div>`;
                }, "width": "40%"
            },
        ],
        "language": {
            "emptyTable":"No data found"
        },
        "width":"100%"
    })
} 

function Delete(url) {
    swal({
        title: "Are you sure to delete this book? ",
        text: "Once you delete it you will not be able to recover",
        icon: "warning",
        buttons: true,
        dangerMode: true
    }).then((willDelete) => {
        if (willDelete) {
            $.ajax({
                type: "DELETE",
                url: url,
                success: function (data) {
                    if (data.success) {
                        toastr.success(data.message);
                        dataTable.ajax.reload();
                    } else {
                        toastr.error(data.message);
                    }
                }
            });
        }
    });
}
```

## Home page
>
> ### Main page 
![Main Page](https://user-images.githubusercontent.com/65743649/128656254-ff4abd8a-8fc2-4207-9a16-7cfd9c376c8a.JPG)

>
>

> ### Create new book.

![Create book](https://user-images.githubusercontent.com/65743649/128656310-24e03228-fd7b-4105-8a74-d67ead5a4102.JPG)

``` cshtml
@page
@model BookListRazor.Pages.BookList.CreateModel

<br />
<h2 class="text-info"> Create New Book </h2>
<br />

<div class="border container" style="width:80%; padding:30px;">
    <form method="post">
        <div class="text-danger" asp-validation-summary="ModelOnly"></div>
        <div class="form-group row">
            <div class="col-2">
                <label asp-for="Book.Name"></label>
            </div>
            <div class="col-6">
                <input asp-for="Book.Name" class="form-control" />
            </div>
            <span asp-validation-for="Book.Name" class="text-danger"></span>
        </div>
        <div class="form-group row">
            <div class="col-2">
                <label asp-for="Book.Author"></label>
            </div>
            <div class="col-6">
                <input asp-for="Book.Author" class="form-control" />
            </div>
            <span asp-validation-for="Book.Author" class="text-danger"></span>
        </div>
        <div class="form-group row">
            <div class="col-2">
                <label asp-for="Book.ISBN"></label>
            </div>
            <div class="col-6">
                <input asp-for="Book.ISBN" class="form-control" />
            </div>
            <span asp-validation-for="Book.ISBN" class="text-danger"></span>
        </div>
        <div class="form-group row">
            <div class="col-2 offset-3">
                <input type="submit" value="Create" class="btn btn-primary form-control" />
            </div>
            <div class="col-2">
                <a asp-page="Index" class="btn btn-success form-control">Back To List</a>
            </div>
        </div>
    </form>
</div>

@section Scripts{
<partial name="_ValidationScriptsPartial" /> 
}

```
>
>
> ### Edit book

![Edit book](https://user-images.githubusercontent.com/65743649/128656451-0191e2a5-22f9-44c5-9e0c-602ae2e1d1c9.JPG)

``` cshtml
@page
@model BookListRazor.Pages.BookList.EditModel


<br />
<h2 class="text-info"> Edit Book </h2>
<br />

<div class="border container" style="width:80%; padding:30px;">
    <form method="post">
        <input type="hidden" asp-for="Book.Id" />
        <div class="text-danger" asp-validation-summary="ModelOnly"></div>
        <div class="form-group row">
            <div class="col-2">
                <label asp-for="Book.Name"></label>
            </div>
            <div class="col-6">
                <input asp-for="Book.Name" class="form-control" />
            </div>
            <span asp-validation-for="Book.Name" class="text-danger"></span>
        </div>
        <div class="form-group row">
            <div class="col-2">
                <label asp-for="Book.Author"></label>
            </div>
            <div class="col-6">
                <input asp-for="Book.Author" class="form-control" />
            </div>
            <span asp-validation-for="Book.Author" class="text-danger"></span>
        </div>
        <div class="form-group row">
            <div class="col-2">
                <label asp-for="Book.ISBN"></label>
            </div>
            <div class="col-6">
                <input asp-for="Book.ISBN" class="form-control" />
            </div>
            <span asp-validation-for="Book.ISBN" class="text-danger"></span>
        </div>
        <div class="form-group row">
            <div class="col-2 offset-3">
                <input type="submit" value="Update" class="btn btn-primary form-control" />
            </div>
            <div class="col-2">
                <a asp-page="Index" class="btn btn-success form-control">Back To List</a>
            </div>
        </div>
    </form>
</div>

@section Scripts{
    <partial name="_ValidationScriptsPartial" />
}

```
>
>

>
>
> ### Delete book

![Delete1](https://user-images.githubusercontent.com/65743649/128656583-944de03a-2097-491d-a655-ec6f4612665b.JPG)

>
>
>### API Contoroller
>
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using BookListRazor.Model;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

namespace BookListRazor.Controllers
{
    [Route("api/book")]
    [ApiController]
    public class BookController : Controller
    {
        private readonly ApplicationDbContext _db;

        public BookController(ApplicationDbContext db)
        {
            _db = db;
        }

        [HttpGet]
        public async Task<IActionResult> GetAll()
        {
            return Json(new { data = await _db.Book.ToListAsync()});
        }

        [HttpDelete]
        public async Task<IActionResult> Delete(int id)
        {
            var bookFromDb = await _db.Book.FirstOrDefaultAsync(u => u.Id == id);
            if(bookFromDb == null)
            {
                return Json(new { success = false, message = "Error while deleting" });
            }
                _db.Book.Remove(bookFromDb);
                await _db.SaveChangesAsync();

            return Json(new { success = true, message = "Delete successful" });
        }
    }
}

```





