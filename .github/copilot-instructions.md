# Repository instructions

## Project overview

- This repository contains a .NET 10 Azure Functions v4 app using the isolated worker model.
- The project file is `AzureFunction.csproj`; nullable reference types and implicit usings are enabled.
- The current function is `HttpTrigger1` in `HttpTrigger1.cs`, in the `Company.Function` namespace. It accepts `GET` and `POST` requests at `AuthorizationLevel.Function` and uses constructor-injected `ILogger<HttpTrigger1>`.
- Application startup is in `Program.cs`. It uses `FunctionsApplication.CreateBuilder`, configures the Functions web application, and conditionally enables OpenTelemetry with the Azure Monitor exporter when `APPLICATIONINSIGHTS_CONNECTION_STRING` is set.

## Common commands

Run these from the repository root:

```powershell
dotnet restore
dotnet build
dotnet publish --configuration Release
```

The VS Code tasks in `.vscode/tasks.json` provide the corresponding clean/build and Release clean/publish workflows. The Functions host task runs from `bin/Debug/net10.0` with:

```powershell
func host start --dotnet-isolated --dotnet-isolated-debug --enable-json-output
```

## Local development

- `host.json` enables OpenTelemetry telemetry mode.
- `local.settings.json` configures the isolated worker runtime and uses Azurite through `AzureWebJobsStorage=UseDevelopmentStorage=true`.
- Do not commit secrets or replace local development settings with production credentials.
- `.vscode/launch.json` attaches the debugger to a running .NET Functions process; start the Functions host before using it.

## Change conventions

- Follow existing C# formatting, nullable annotations, dependency injection, and `ILogger` patterns.
- Preserve the isolated-worker hosting model and the existing Function attribute/trigger conventions when adding functions.
- Keep Application Insights/OpenTelemetry registration conditional on `APPLICATIONINSIGHTS_CONNECTION_STRING` unless the behavior is intentionally changed.
- Build the project after code changes. There is currently no test project or repository lint configuration, so do not assume additional test or lint commands exist.
- Do not edit generated `bin` or `obj` contents directly.