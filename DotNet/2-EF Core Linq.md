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

### Ejemplos aplicados

* **hacer top 1**

   ```csharp
   var registro = context.Productos
                        .OrderBy(p => p.Precio)
                        .FirstOrDefault();
   ```
* **hacer top 1 desc**

```csharp
var ultimo = context.Productos
                    .OrderByDescending(p => p.FechaCreacion)
                    .FirstOrDefault();
```

* **Obtener solo una columna**

```csharp
var year = await _context.PeriodoContable
         .Where(e=> e.EmpresaId == EmpresaId)
         .OrderByDescending(p => p.Anio)
         .Select(p => p.Anio)
         .FirstOrDefaultAsync();
```

### Resumen De Metodos EF Core

### Métodos de Ejecución y Recuperación en EF Core

* **Ejecuta y trae todos los resultados como una lista**
    * Comando: `ToList()` / `ToListAsync()`
    * Ejemplo: `await db.Productos.Where(p => p.Stock > 0).ToListAsync()`
    * Retorno: `List<T>`

* **Retorna el primer elemento, o `null` si no encuentra ninguno**
    * Comando: `FirstOrDefault()` / `FirstOrDefaultAsync()`
    * Ejemplo: `await db.Pedidos.FirstOrDefaultAsync(p => p.Id == 10)`
    * Retorno: `T` o `null`

* **Retorna el único elemento; lanza excepción si es cero o más de uno**
    * Comando: `Single()` / `SingleAsync()`
    * Ejemplo: `db.Configuracion.Single(c => c.Activa == true)`
    * Retorno: `T`

* **Busca por clave primaria, primero en memoria y luego en la base de datos**
    * Comando: `Find()` / `FindAsync()`
    * Ejemplo: `await db.Clientes.FindAsync(customerId)`
    * Retorno: `T` o `null`

* **Ejecuta y trae todos los resultados como un array**
    * Comando: `ToArray()` / `ToArrayAsync()`
    * Ejemplo: `await db.Usuarios.OrderBy(u => u.Nombre).ToArrayAsync()`
    * Retorno: `T[]`

###  Métodos de Agregación y Conteo en EF Core

* **Retorna el número total de elementos que cumplen la condición**
    * Comando: `Count()` / `CountAsync()`
    * Ejemplo: `await db.Items.CountAsync(i => i.Estado == "Activo")`
    * Retorno: `int`

* **Similar a Count, pero para conjuntos de datos muy grandes**
    * Comando: `LongCount()` / `LongCountAsync()`
    * Ejemplo: `db.Logs.LongCount()`
    * Retorno: `long`

* **Calcula la suma de los valores de una propiedad numérica**
    * Comando: `Sum()` / `SumAsync()`
    * Ejemplo: `await db.Ordenes.SumAsync(o => o.Total)`
    * Retorno: Tipo numérico

* **Calcula el promedio de los valores de una propiedad numérica**
    * Comando: `Average()` / `AverageAsync()`
    * Ejemplo: `db.Notas.Average(n => n.Puntuacion)`
    * Retorno: Tipo numérico

* **Determina el valor mínimo de una propiedad en la secuencia**
    * Comando: `Min()` / `MinAsync()`
    * Ejemplo: `await db.Productos.MinAsync(p => p.Precio)`
    * Retorno: Tipo

* **Determina el valor máximo de una propiedad en la secuencia**
    * Comando: `Max()` / `MaxAsync()`
    * Ejemplo: `db.Empleados.Max(e => e.Salario)`
    * Retorno: Tipo


### Métodos Condicionales en EF Core

* **Determina si al menos un elemento satisface la condición**
    * Comando: `Any()` / `AnyAsync()`
    * Ejemplo: `await db.Usuarios.AnyAsync(u => u.Rol == "Admin")`
    * Retorno: `bool`

* **Determina si todos los elementos satisfacen la condición**
    * Comando: `All()` / `AllAsync()`
    * Ejemplo: `db.Tareas.All(t => t.Prioridad > 0)`
    * Retorno: `bool`

* **Determina si una secuencia contiene un elemento específico**
    * Comando: `Contains()`
    * Ejemplo: `db.Categorias.Contains(miCategoria)`
    * Retorno: `bool`

### Proyection y transformacion

* **Transforma o proyecta la secuencia en un nuevo formulario (columnas u objeto)**
    * Comando: `Select()`
    * Ejemplo: `db.Autores.Select(a => a.NombreCompleto)`
    * Retorno: `IQueryable<TResult>`

* **Proyecta y aplana colecciones anidadas en una sola secuencia**
    * Comando: `SelectMany()`
    * Ejemplo: `db.Blogs.SelectMany(b => b.Posts)`
    * Retorno: `IQueryable<TResult>`

### Filtrado

* **Filtra la secuencia de valores basada en una condición (predicado)**
    * Comando: `Where()`
    * Ejemplo: `db.Libros.Where(l => l.Publicado > 2020)`
    * Retorno: `IQueryable<T>`

* **Filtra elementos basados en un tipo específico (útil para herencia)**
    * Comando: `OfType<TResult>()`
    * Ejemplo: `db.Entidades.OfType<EntidadHija>()`
    * Retorno: `IQueryable<TResult>`


### Ordenamiento

* **Ordena los elementos en orden ascendente o descendente**
    * Comando: `OrderBy()` / `OrderByDescending()`
    * Ejemplo: `db.Articulos.OrderBy(a => a.Fecha)`
    * Retorno: `IOrderedQueryable<T>`

* **Define un ordenamiento secundario (debe seguir a OrderBy)**
    * Comando: `ThenBy()` / `ThenByDescending()`
    * Ejemplo: `db.Personas.OrderBy(p => p.Apellido).ThenBy(p => p.Nombre)`
    * Retorno: `IOrderedQueryable<T>`


### Paginacion

* **Omite un número especificado de elementos al inicio**
    * Comando: `Skip()`
    * Ejemplo: `db.Comentarios.Skip(20)`
    * Retorno: `IQueryable<T>`

* **Devuelve un número especificado de elementos al inicio**
    * Comando: `Take()`
    * Ejemplo: `db.Productos.Take(10)`
    * Retorno: `IQueryable<T>`


### Relaciones (Eager Loading)

* **Incluye una propiedad de navegación (relación) en la consulta**
    * Comando: `Include()`
    * Ejemplo: `db.Pedidos.Include(p => p.Cliente)`
    * Retorno: `IQueryable<T>`

* **Encadena la inclusión para cargar relaciones anidadas (más profundas)**
    * Comando: `ThenInclude()`
    * Ejemplo: `db.Pedidos.Include(p => p.Cliente).ThenInclude(c => c.Direccion)`
    * Retorno: `IIncludableQueryable<T, TProperty>`

### Modificadores de Consulta

* **Indica al contexto no rastrear entidades (para consultas de solo lectura)**
    * Comando: `AsNoTracking()`
    * Ejemplo: `db.Reportes.AsNoTracking().ToList()`
    * Retorno: `IQueryable<T>`

* **Agrupa los elementos que comparten una clave en común**
    * Comando: `GroupBy()`
    * Ejemplo: `db.Ventas.GroupBy(v => v.Mes)`
    * Retorno: `IQueryable<IGrouping<TKey, TElement>>`

* **Retorna solo los elementos únicos o distintos**
    * Comando: `Distinct()`
    * Ejemplo: `db.Correos.Select(c => c.Dominio).Distinct()`
    * Retorno: `IQueryable<T>`


### guardar nuevo elemento

## Data Annotations en EF Core

* **Indica que la propiedad es la clave primaria (PK)**
    * Comando: `[Key]`
    * Ejemplo: `[Key] public int IdRol { get; set; }`

* **Define el nombre de la tabla en la base de datos**
    * Comando: `[Table("NombreTabla")]`
    * Ejemplo: `[Table("Roles")]`

* **Define el nombre de la columna en la tabla**
    * Comando: `[Column("NombreColumna")]`
    * Ejemplo: `[Column("DescripcionRol")]`

* **Especifica el tipo de dato SQL**
    * Comando: `[Column(TypeName = "...")]`
    * Ejemplo: `[Column(TypeName = "decimal(18,2)")]`

* **Indica que el campo es obligatorio (NOT NULL)**
    * Comando: `[Required]`
    * Ejemplo: `[Required] public string Nombre { get; set; }`

* **Define el tamaño máximo de la cadena**
    * Comando: `[MaxLength(n)]`
    * Ejemplo: `[MaxLength(100)]`

* **Define tamaño máximo y mínimo de la cadena**
    * Comando: `[StringLength(n)]`
    * Ejemplo: `[StringLength(100, MinimumLength = 3)]`

* **Define la longitud mínima de la cadena**
    * Comando: `[MinLength(n)]`
    * Ejemplo: `[MinLength(3)]`

* **Define precisión y escala para valores numéricos o decimales**
    * Comando: `[Precision(p, s)]`
    * Ejemplo: `[Precision(10, 2)] public decimal Precio { get; set; }`

* **Define la clave foránea (FK) hacia otra entidad**
    * Comando: `[ForeignKey("PropiedadNavegacion")]`
    * Ejemplo: `[ForeignKey("Rol")] public int IdRol { get; set; }`

* **Indica la propiedad inversa en una relación**
    * Comando: `[InverseProperty("PropiedadRelacionada")]`
    * Ejemplo: `[InverseProperty("Usuarios")]`

* **Excluye la propiedad del mapeo a la base de datos**
    * Comando: `[NotMapped]`
    * Ejemplo: `[NotMapped] public string NombreCompleto => Nombre + " " + Apellido;`

* **Marca el campo para control de concurrencia**
    * Comando: `[ConcurrencyCheck]`
    * Ejemplo: `[ConcurrencyCheck] public string Email { get; set; }`

* **Crea una columna de versión binaria para control de concurrencia**
    * Comando: `[Timestamp]`
    * Ejemplo: `[Timestamp] public byte[] RowVersion { get; set; }`

* **Crea un índice (único o no)**
    * Comando: `[Index(...)]`
    * Ejemplo: `[Index(nameof(Email), IsUnique = true)]`

* **Define si la columna es Unicode o no (nvarchar o varchar)**
    * Comando: `[Unicode(false)]`
    * Ejemp*

