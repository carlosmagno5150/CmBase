# Copilot Instructions

## Repository Overview

This is a small .NET 10 ASP.NET Core solution. It contains:

- `src/CMBaseApi` - an ASP.NET Core Web API application.
- `tests/CMBaseTests` - an xUnit test project.
- `cmbase.slnx` - the solution file.

The API uses ASP.NET Core controller-based routing. `Program.cs` registers controllers and OpenAPI, starts the web host, and maps controller endpoints. The current endpoint is a weather forecast sample implemented in `WeatherForecastController`.

## Build, Run, and Test

Run commands from the repository root.

```powershell
# Restore/build everything
dotnet restore .\cmbase.slnx
dotnet build .\cmbase.slnx

# Run the API
dotnet run --project .\src\CMBaseApi\CMBaseApi.csproj
```

The launch profile serves the API at `http://localhost:5114`. OpenAPI is mapped when `ASPNETCORE_ENVIRONMENT=Development`.

The repository has no separate lint or format script/configuration. Do not assume one exists; use `dotnet build` for the normal compile check.

```powershell
# Run the full test project
dotnet test .\tests\CMBaseTests\CMBaseTests.csproj

# Run one test by fully qualified name
dotnet test .\tests\CMBaseTests\CMBaseTests.csproj --filter "FullyQualifiedName=CMBaseTests.UnitTest1.Test1"

# Run tests matching a name filter
dotnet test .\tests\CMBaseTests\CMBaseTests.csproj --filter "Name~Test1"
```

The test project targets `net10.0` and uses xUnit 2.9.3 with the Visual Studio runner adapter. The API is not currently referenced by the test project.

## Architecture and Request Flow

- `src/CMBaseApi/Program.cs` is the composition root. Service registration and HTTP pipeline configuration live there.
- `src/CMBaseApi/Controllers/` contains MVC controller endpoints. Controllers use `[ApiController]` and attribute-based `[Route]`/HTTP verb attributes.
- `src/CMBaseApi/CMBaseApi.http` contains a sample HTTP requests.
- `launchSettings.json` defines the `http` launch profile and sets the development environment.
- Use Vertical Slice Architecture for each Application, Domain, Data folders. 


## Code Conventions

- Target .NET 10 with nullable reference types enabled and implicit usings enabled in both projects.
- Keep controller classes under `CMBaseApi.Controllers` and route controllers relative to their class name unless the API contract requires another route.
- Use ASP.NET Core attributes (`[ApiController]`, `[HttpGet]`, `[Route]`) for endpoint metadata.
- Existing C# code uses namespace block style and collection expressions (for example, `string[]` initialization with `[...]`). Preserve that style in nearby code.
- Add xUnit tests as public classes with `[Fact]` methods. The test project's global xUnit using makes `Fact` available without an explicit `using Xunit;`.
- OpenAPI is only mapped in the Development environment. Configuration belongs in `appsettings.json` or `appsettings.Development.json` rather than as hard-coded settings.
