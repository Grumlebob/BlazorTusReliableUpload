# Blazor Tus Reliable Upload

Small Blazor Web App sample showing reliable image upload with `tusdotnet`, Blazor Interactive Auto, EF Core, PostgreSQL, Redis, and Docker Compose.

This repository is copied from `BlazorAutoAppTemplate`, but intentionally excludes GitHub Actions CI/CD and the deployment folder. It is meant to be a local development and sample repo, not a production deployment package.

## Start Here

- `HowToRunLocally.md` explains local Docker and developer setup.
- `HullImages.md` documents the hull image upload feature.
- `overview.md` is the deeper architecture walkthrough.

## Tech Stack

- .NET 10 Blazor Web App with `InteractiveAuto`.
- EF Core and Npgsql for PostgreSQL.
- ASP.NET Core Identity on the application `AppDbContext`.
- Redis for HybridCache and Data Protection keys.
- Serilog console logging and Seq in local Docker.
- `tusdotnet` for resumable uploads.

## Repository Layout

- `BlazorAutoApp.Core/Features/*` contains vertical slices with contracts, entities, requests, and responses.
- `BlazorAutoApp` is the server host, EF Core owner, Minimal API owner, and SSR/prerender runtime.
- `BlazorAutoApp.Client` is the WASM client loaded after hydration.
- `BlazorAutoApp.Test` contains xUnit tests and architecture checks.
- `docker-compose.yml` runs the local app stack.

## Architecture

Core defines shared feature contracts such as `IHullImagesApi` and `IMoviesApi`. The server implements those contracts with EF Core and exposes Minimal API endpoints. The WASM client implements the same contracts with `HttpClient`.

Pages depend on Core contracts, so the same UI works during server prerender and after WASM hydration. Pages use `PersistentComponentState` to avoid duplicate fetches when transitioning from SSR to interactive rendering.

Identity endpoints:

- Login: `/Identity/Account/Login`
- Register: `/Identity/Account/Register`
- Public showcase: `/api/identity-showcase/public`
- Authorized showcase: `/api/identity-showcase/secure`

## Testing

Docker must be running because integration tests use Testcontainers.

```powershell
dotnet test
```
