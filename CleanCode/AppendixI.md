# Swagger / Open API practices

## Get start with Swashbuckle
### Install the package
### Configure Swagger
* Add Swagger Service
```java
services.AddSwaggerGen(c =>
{
	c.SwaggerDoc("v1", new OpenApiInfo
	{
        Version = "v1",
        Title = "Books API",
        Description = "A simple example ASP.NET Core Web API",
        TermsOfService = new Uri("https://example.com/terms"),
        Contact = new OpenApiContact
        {
            Name = "Shayne Boyer",
            Email = string.Empty,
            Url = new Uri("https://twitter.com/spboyer")
        },
        License = new OpenApiLicense
        {
            Name = "Use under LICX",
            Url = new Uri("https://example.com/license")
        }
	});
	
    // Set the comments path for the Swagger JSON and UI.
    var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
    var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
    c.IncludeXmlComments(xmlPath);
});
```
* Use Swagger
```java
app.UseSwagger();
app.UseSwaggerUI(c =>
{
    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
    //To make Swagger UI appear from the root path
    c.RoutePrefix = string.Empty;
});
```
* Enable XML Comments to show doc comments api in Swagger, add following to _.csproj_
```java
<PropertyGroup>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```
* Specify Json Formatting
By default, json formatting will be camel form, you can specify the formatting to make it same as member name.
```java
services.AddControllers().AddNewtonsoftJson(options => options.UseMemberCasing());
```
* Add Open API analyzer
```java
<PropertyGroup>
 <IncludeOpenAPIAnalyzers>true</IncludeOpenAPIAnalyzers>
</PropertyGroup>
```

### A Good Example of Swagger Header
```java
[Produces("application/json")]
[Route("api/[controller]")]
[ApiController]
public class BooksController : ControllerBase {
    /// <summary>
    /// Creates a Book
    /// </summary>
    /// <remarks>
    /// Sample request:
    /// 
    ///     POST /Book
    ///     {
    ///         "name" : "Item1",
    ///         "price" : "12.5",
    ///         "category" : "comedy",
    ///         "author" : "unknown"
    ///     }
    ///     
    /// </remarks>
    /// <param name="book"></param>
    /// <returns>A newly created Book</returns>
    /// <response code="201">Returns the newly created item</response>
    /// <response code="400">If the item is null</response>
    [HttpPost]
    [ProducesResponseType(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public ActionResult<Book> Create(Book book)
    {
    	_bookService.Create(book);

    	return CreatedAtRoute("GetBook", new { id = book.Id.ToString() }, book);
    }
}

```