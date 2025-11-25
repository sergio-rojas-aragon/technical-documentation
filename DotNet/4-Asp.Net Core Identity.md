---
title: ASP.NET Core Identity
layout: home
parent: DotNet
nav_order: 4
---

# Relacion entre tablas creadas automaticamente

para hacer una relacion entre AspNetUsers y una tabla custom teniendo dos dbcontext, uno para el auth y otro para las tablas custom que se podria llamar AppDbContext, se tiene que dejar el AppDbContext como sigue:

```
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using TUM.Domain.Entities;


namespace TUM.Infrastructure.Persistence
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        public DbSet<Pedido> Pedidos => Set<Pedido>();
        ...

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
           
            // evito el intento de duplicacion de aspnetusers
            
            modelBuilder.Entity<IdentityUser>()
    .           ToTable("AspNetUsers", t => t.ExcludeFromMigrations());

            base.OnModelCreating(modelBuilder);


            // creo la relacion entre pedido y aspnetusers

            modelBuilder.Entity<Pedido>()
                .HasOne<IdentityUser>() // sin navegación en dominio
                .WithMany()
                .HasForeignKey(p => p.CreatedBy)
                .OnDelete(DeleteBehavior.Restrict);

        }
    }
}
```


# ASP.NET Core Identity con Identity API Endpoints

Para `.net8`


# Flujo

## Registrar usuario (Sign Up)

Endpoint: POST /register

JSON body:

```
{
  "email": "admin@test.com",
  "password": "Admin123!"
}
```

✔ Crea el usuario
✔ Lo guarda en AspNetUsers
Respuesta esperada:

```
{
  "userId": "fb1c0c8e-be96-4ad2-a467-0f97096ba22e"
}
```

## Iniciar sesión (Login)

Endpoint: POST /login?useCookies=true

O si quieres token JWT:

```
POST /login?useCookie=false
```

Body:

```
{
  "email": "admin@test.com",
  "password": "Admin123!"
}
```
Si usas cookies

La respuesta contiene un Set-Cookie:

Set-Cookie: .AspNetCore.Identity.Application=…

Con eso ya estás autenticado.
Si usas token:

La respuesta contiene un JWT:

```
{
  "token": "eyJhbGciOiJIUzI1NiIsInR..."
}
```

## Obtener el usuario actual

Una vez logeado:

GET /me

Devuelve:

```
{
  "id": "fb1c0c8e-be96-4ad2-a467-0f97096ba22e",
  "email": "admin@test.com"
}
```

## Cerrar sesión

POST /logout

Si usabas cookies, borra la cookie.
Si usabas JWT… no hace nada (los JWT no se invalidan).


## Proteger tus propios endpoints

En tu API puedes hacer:

app.MapGet("/tareas", () => "Solo usuarios logeados")
    .RequireAuthorization();

Si intentas llamar sin login → 401 Unauthorized.



🚀 Identity API Endpoints (.NET 8) — Documentación Completa

Esta API utiliza ASP.NET Core Identity con Identity API Endpoints, generados automáticamente al usar: