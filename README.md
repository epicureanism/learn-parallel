# parallel

# .Net Core DI

## Service lifetimes

Services can be registered with one of the following lifetimes:

### Transient
  
Transient lifetime services are created each time they're requested from the service container. This lifetime works best for lightweight, stateless services. Register transient services with AddTransient.

In apps that process requests, transient services are disposed at the end of the request.

### Scoped
  
Scoped lifetime services are created once per client request (connection). Register scoped services with AddScoped.

In apps that process requests, scoped services are disposed at the end of the request.

When using Entity Framework Core, the AddDbContext extension method registers DbContext types with a scoped lifetime by default.

Do not resolve a scoped service from a singleton. It may cause the service to have incorrect state when processing subsequent requests. It's fine to:

Resolve a singleton service from a scoped or transient service.
Resolve a scoped service from another scoped or transient service.

### Singleton
  
Singleton lifetime services are created either:

    - The first time they're requested.
    - By the developer, when providing an implementation instance directly to the container. This approach is rarely needed.
Every subsequent request uses the same instance. If the app requires singleton behavior, allow the service container to manage the service's lifetime. Don't implement the singleton design pattern and provide code to dispose of the singleton. Services should never be disposed by code that resolved the service from the container. If a type or factory is registered as a singleton, the container disposes the singleton automatically.

Register singleton services with AddSingleton. Singleton services must be thread safe and are often used in stateless services.

In apps that process requests, singleton services are disposed when the ServiceProvider is disposed on application shutdown. Because memory is not released until the app is shut down, **consider memory use with a singleton service**.

