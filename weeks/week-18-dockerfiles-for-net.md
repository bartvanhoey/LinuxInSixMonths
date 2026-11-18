[← Week 17](week-17-docker-fundamentals.md) | [Overview](../README.md) | [Week 19 →](week-19-docker-compose.md)

# Week 18 - Dockerfiles for .NET

*Phase 5: Containers (Weeks 17-20)*

- [ ] **Done**

**Goal:** Build small, fast and safe images for your apps.

**Exercises:**

- [ ] Write a multi-stage Dockerfile using the official mcr.microsoft.com/dotnet sdk and aspnet images.
- [ ] Optimise layer caching (copy csproj and restore first), add .dockerignore, run as the non-root app user.
- [ ] Add a HEALTHCHECK and an environment-based configuration.
- [ ] Compare image sizes: sdk base, aspnet, alpine, chiseled.

**Deliverable:** Dockerfile for your Blazor app with the image size before and after optimisation.

---

[← Week 17](week-17-docker-fundamentals.md) | [Overview](../README.md) | [Week 19 →](week-19-docker-compose.md)
