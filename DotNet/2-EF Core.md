---
title: EF NetCore y Linq
layout: home
parent: DotNet
nav_order: 2
---

# Entity Framework Core
{: .no_toc }

Apuntes que tomo cuando utilizo Entity Framework Core
{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Instalacion

```terminal
dotnet tool install --global dotnet-ef
```

## Migraciones

Cuando se utilizan las migraciones, si existen datos y se modifica las tablas, se trata de mantener la data adaptando las tablas modificadas. Si el cambio es muy grande se tiene que eliminar toda la Base de datos, ya que la migracion fallara al no poder realizar el cambio necesario en la informacion.

### Iniciar migraciones y tambien aplica para posteriores

Donde dice Incial es el nombre de la migracion, por lo que puede ser el nombre que uno quiera:

```terminal
dotnet ef migrations add Inicial
```

* **actualizar migraciones**
    ```terminal
    dotnet ef database update
    ```

### listar migraciones

```terminal
dotnet ef migrations list
```

### revertir migracion a un paso anterior

el nombre de las migraciones tiene fecha, es parte del nombre asi que se tiene que incluir
Esto puede fallar

```terminal
dotnet ef database update nombreCompletoMigracion
```


### Eliminar migraciones

```terminal
dotnet ef migrations remove
```

### Crear tabla con datos

Se tiene que ingresar la data en el DbContext usando hasData. Al hacer update en la migracion creara las tablas y ademas le agregara los datos.
Se tiene que programar el HasData antes de que se genere la migracion.

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Rol>().HasData(
        new Rol { RolId = 1, Nombre = "Administador" },
        new Rol { RolId = 2, Nombre = "Empleado" },
        new Rol { RolId = 3, Nombre = "Vendedor" }
        );
}
```

## Uso con distintos DbContexts y arquitecturas

se tiene que especificar a que dbContext se quiere realizar la migracion

### List from AppDbContext and DDD architecture

```terminal
dotnet ef migrations list --context AppDbContext --project ./src/TUM.Infrastructure --startup-project ./src/TUM.Api
```

### Create a migration from AppDbContext and DDD architecture

```terminal
dotnet ef migrations add PedidoCreatedByFK --context AppDbContext --project src/TUM.Infrastructure --startup-project src/TUM.Api
```

### enviar cambios

```terminal
dotnet ef database update --context AppDbContext --project src/TUM.Infrastructure --startup-project src/TUM.Api
```

## Convenciones importantes

### nombres de tablas

los objetos del Models deben ser en singular (por ejemplo Rol en vez de Roles).
Cuando EF Core las migra a base de datos las pluraliza, es decir le coloca Roles.


### Nombres de los Ids

pueden ser Id o RolId para que EF Core pueda identificarla automaticamente por convencion. No puede Ser idRol.

En las relaciones si puede ser plural.


## Relaciones de tablas

## uno a muchos

Un rol puede tener muchos usuarios

```csharp
public class Rol
{
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int RolId { get; set; }
    [Required]
    public string Nombre { get; set; }

    // Relacion uno a muchos
    public ICollection<Usuario> Usuarios { get; set; } = new List<Usuario>();

}
```

```csharp
public class Usuario
{
    public int UsuarioId { get; set; }
    [Required]
    public string Nombre { get; set; } = string.Empty;
    [Required]
    [EmailAddress]
    public string Email { get; set; } = string.Empty;

    // claves foraneas
    public int RolId { get; set; }
    public Rol Rol { get; set; } = null!;

}
```

## Operaciones

### buscar by id


### Resumen De Metodos EF Core

### Métodos de Ejecución y Recuperación en EF Core

* **`ToList()` / `ToListAsync()`**
    * Ejecuta y trae **todos** los resultados como una lista. 
    * Retorno: `List<T>`
    * Ejemplo: `await db.Productos.Where(p => p.Stock > 0).ToListAsync()`

* **`FirstOrDefault()` / `FirstOrDefaultAsync()`**
    * Retorna el **primer** elemento, o `null` si no encuentra ninguno. 
    * Retorno: `T` o `null` 
    * Ejemplo: `await db.Pedidos.FirstOrDefaultAsync(p => p.Id == 10)`

* **`Single()`/`SingleAsync()`** 
    * Retorna el **único** elemento; lanza excepción si es cero o más de uno.
    * Ejemplo: `db.Configuracion.Single(c => c.Activa == true)` 
    * Retorno:`T`

* **`Find()` / `FindAsync()`**  
    * Busca por **clave primaria**, primero en memoria y luego en la DB. 
    * Ejemplo: `await db.Clientes.FindAsync(customerId)`
    * Retorno:`T` o `null` 

* **`ToArray()` / `ToArrayAsync()`**
    * Ejecuta y trae **todos** los resultados como un array.
    * Ejemplo:  `await db.Usuarios.OrderBy(u => u.Nombre).ToArrayAsync()`
    * Retorno: `T[]` 

###  Métodos de Agregación y Conteo en EF Core


* **`Count()` / `CountAsync()`**
    * Retorno: `int`
    * Retorna el número **total** de elementos que cumplen la condición.
    * Ejemplo: `await db.Items.CountAsync(i => i.Estado == "Activo")`4

* **`LongCount()`/`LongCountAsync()`**
    * Retorno:`long`
    * Similar a `Count`, pero para conjuntos de datos **muy grandes**.
    * Ejemplo: `db.Logs.LongCount()`

* **`Sum()`/`SumAsync()`**
    * Retorno:Tipo numérico
    * Calcula la **suma** de los valores de una propiedad numérica.
    * Ejemplo: `await db.Ordenes.SumAsync(o => o.Total)`

* **`Average()`/`AverageAsync()`**
    * Retorno:Tipo numérico
    * Calcula el **promedio** de los valores de una propiedad numérica.
    * Ejemplo: `db.Notas.Average(n => n.Puntuacion)`

* **`Min()`/`MinAsync()`**
    * Retorno: Tipo
    * Determina el valor **mínimo** de una propiedad en la secuencia.
    * Ejemplo: `await db.Productos.MinAsync(p => p.Precio)`

* **`Max()`/`MaxAsync()`**
    * Retorno: Tipo
    * Determina el valor **máximo** de una propiedad en la secuencia.
    * Ejemplo: `db.Empleados.Max(e => e.Salario)`

### Métodos Condicionales en EF Core

* **Any() / AnyAsync()** - Determina si **al menos un** elemento satisface la condición.
   * Ejemplo: `await db.Usuarios.AnyAsync(u => u.Rol == "Admin")`
   * Retorno: `bool`

* **All() / AllAsync()** - Determina si **todos** los elementos satisfacen la condición.
   * Ejemplo: `db.Tareas.All(t => t.Prioridad > 0)`
   * Retorno: `bool`

* **Contains()** - Determina si una secuencia **contiene** un elemento específico.
   * Ejemplo: `db.Categorias.Contains(miCategoria)`
   * Retorno: `bool`


### Proyection y transformacion

* **Select()** - Transforma o proyecta la secuencia en un **nuevo formulario** (columnas/objeto).
   * Ejemplo: `db.Autores.Select(a => a.NombreCompleto)`
   * Retorno: `IQueryable<TResult>`

* **SelectMany()** - Proyecta y **aplana** colecciones anidadas en una sola secuencia.
   * Ejemplo: `db.Blogs.SelectMany(b => b.Posts)`
   * Retorno: `IQueryable<TResult>`


### FIltrado

* **Where()** - Filtra la secuencia de valores basada en una **condición** (predicado).
   * Ejemplo: `db.Libros.Where(l => l.Publicado > 2020)`
   * Retorno: `IQueryable<T>`

* **OfType<TResult>()** - Filtra elementos basados en un **tipo específico** (útil para herencia).
   * Ejemplo: `db.Entidades.OfType<EntidadHija>()`
   * Retorno: `IQueryable<TResult>`


### Ordenamiento

* **OrderBy() / OrderByDescending()** - Ordena los elementos en orden **ascendente** o **descendente**.
   * Ejemplo: `db.Articulos.OrderBy(a => a.Fecha)`
   * Retorno: `IOrderedQueryable<T>`

* **ThenBy() / ThenByDescending()** - Define un **ordenamiento secundario** (debe seguir a `OrderBy`).
   * Ejemplo: `db.Personas.OrderBy(p => p.Apellido).ThenBy(p => p.Nombre)`
   * Retorno: `IOrderedQueryable<T>`


### Paginacion

* **Skip()** - **Omite** un número especificado de elementos al inicio.
   * Ejemplo: `db.Comentarios.Skip(20)`
   * Retorno: `IQueryable<T>`

* **Take()** - Devuelve un número **especificado** de elementos al inicio.
   * Ejemplo: `db.Productos.Take(10)`
   * Retorno: `IQueryable<T>`


### Relaciones (Eager Loading)

* **Include()** - Incluye una **propiedad de navegación** (relación) en la consulta.
   * Ejemplo: `db.Pedidos.Include(p => p.Cliente)`
   * Retorno: `IQueryable<T>`

* **ThenInclude()** - Encadena la inclusión para cargar **relaciones anidadas** (más profundas).
   * Ejemplo: `db.Pedidos.Include(p => p.Cliente).ThenInclude(c => c.Direccion)`
   * Retorno: `IIncludableQueryable<T, TProperty>`


### Modificadores de Consulta

* **AsNoTracking()** - Indica al contexto **no rastrear** entidades (para consultas de solo lectura).
   * Ejemplo: `db.Reportes.AsNoTracking().ToList()`
   * Retorno: `IQueryable<T>`

* **GroupBy()** - **Agrupa** los elementos que comparten una clave en común.
   * Ejemplo: `db.Ventas.GroupBy(v => v.Mes)`
   * Retorno: `IQueryable<IGrouping<TKey, TElement>>`

* **Distinct()** - Retorna solo los elementos **únicos** o distintos.
   * Ejemplo: `db.Correos.Select(c => c.Dominio).Distinct()`
   * Retorno: `IQueryable<T>`



### guardar nuevo elemento


## Data Annotations en EF Core


* **[Key]** - Indica que la propiedad es la **clave primaria (PK)**.
   * Ejemplo: `[Key] public int IdRol { get; set; }`

* **[Table("NombreTabla")]** - Define el **nombre de la tabla** en la base de datos.
   * Ejemplo: `[Table("Roles")]`

* **[Column("NombreColumna")]** - Define el **nombre de la columna** en la tabla.
   * Ejemplo: `[Column("DescripcionRol")]`

* **[Column(TypeName = "...")]** - Especifica el **tipo de dato SQL**.
   * Ejemplo: `[Column(TypeName = "decimal(18,2)")]`

* **[Required]** - Indica que el campo es **obligatorio** (`NOT NULL`).
   * Ejemplo: `[Required] public string Nombre { get; set; }`

* **[MaxLength(n)]** - Define el **tamaño máximo** de la cadena.
   * Ejemplo: `[MaxLength(100)]`

* **[StringLength(n)]** - Similar a `MaxLength`, pero también puede definir **mínimo**.
   * Ejemplo: `[StringLength(100, MinimumLength = 3)]`

* **[MinLength(n)]** - Define la **longitud mínima** de la cadena.
   * Ejemplo: `[MinLength(3)]`

* **[Precision(p, s)]** - Define **precisión y escala** para valores numéricos o decimales.
   * Ejemplo: `[Precision(10, 2)] public decimal Precio { get; set; }`

* **[ForeignKey("PropiedadNavegacion")]** - Define la **clave foránea (FK)** hacia otra entidad.
   * Ejemplo: `[ForeignKey("Rol")] public int IdRol { get; set; }`

* **[InverseProperty("PropiedadRelacionada")]** - Indica la **propiedad inversa** en una relación.
   * Ejemplo: `[InverseProperty("Usuarios")]`

* **[NotMapped]** - Excluye la propiedad del mapeo a la base de datos.
   * Ejemplo: `[NotMapped] public string NombreCompleto => Nombre + " " + Apellido;`

* **[ConcurrencyCheck]** - Marca el campo para **control de concurrencia**.
   * Ejemplo: `[ConcurrencyCheck] public string Email { get; set; }`

* **[Timestamp]** - Crea una **columna de versión binaria** para control de concurrencia.
   * Ejemplo: `[Timestamp] public byte[] RowVersion { get; set; }`

* **[Index(...)]** - Crea un **índice** (único o no).
   * Ejemplo: `[Index(nameof(Email), IsUnique = true)]`

* **[Unicode(false)]** - Define si la columna es **Unicode o no** (`nvarchar` o `varchar`).
   * Ejemplo: `[Unicode(false)] public string Codigo { get; set; }`

* **[Comment("Texto")]** - Agrega un **comentario** a nivel de columna o tabla en la BD.
   * Ejemplo: `[Comment("Fecha de creación del registro")]`


