---
title: Entity Framework Core
layout: home
parent: DotNet
nav_order: 2
---

# Entity Framework Core


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

### actualizar migraciones

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

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Rol>().HasData(
        new Rol { RolId = 1, Nombre = "Administador" },
        new Rol { RolId = 2, Nombre = "Empleado" },
        new Rol { RolId = 3, Nombre = "Vendedor" }
        );
}
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

```
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

```
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

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`ToList()`/`ToListAsync()`** | `List<T>` | Ejecuta y trae **todos** los resultados como una lista. | `await db.Productos.Where(p => p.Stock > 0).ToListAsync()` |
| **`FirstOrDefault()`<br>`FirstOrDefaultAsync()`** | `T` o `null` | Retorna el **primer** elemento, o `null` si no encuentra ninguno. | `await db.Pedidos.FirstOrDefaultAsync(p => p.Id == 10)` |
| **`Single()`/`SingleAsync()`** | `T` | Retorna el **único** elemento; lanza excepción si es cero o más de uno. | `db.Configuracion.Single(c => c.Activa == true)` |
| **`Find()`/`FindAsync()`** | `T` o `null` | Busca por **clave primaria**, primero en memoria y luego en la DB. | `await db.Clientes.FindAsync(customerId)` |
| **`ToArray()`/`ToArrayAsync()`** | `T[]` | Ejecuta y trae **todos** los resultados como un array. | `await db.Usuarios.OrderBy(u => u.Nombre).ToArrayAsync()` |

###  Métodos de Agregación y Conteo en EF Core

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`Count()`/`CountAsync()`** | `int` | Retorna el número **total** de elementos que cumplen la condición. | `await db.Items.CountAsync(i => i.Estado == "Activo")` |
| **`LongCount()`/`LongCountAsync()`** | `long` | Similar a `Count`, pero para conjuntos de datos **muy grandes**. | `db.Logs.LongCount()` |
| **`Sum()`/`SumAsync()`** | Tipo numérico | Calcula la **suma** de los valores de una propiedad numérica. | `await db.Ordenes.SumAsync(o => o.Total)` |
| **`Average()`/`AverageAsync()`** | Tipo numérico | Calcula el **promedio** de los valores de una propiedad numérica. | `db.Notas.Average(n => n.Puntuacion)` |
| **`Min()`/`MinAsync()`** | Tipo | Determina el valor **mínimo** de una propiedad en la secuencia. | `await db.Productos.MinAsync(p => p.Precio)` |
| **`Max()`/`MaxAsync()`** | Tipo | Determina el valor **máximo** de una propiedad en la secuencia. | `db.Empleados.Max(e => e.Salario)` |

### Métodos Condicionales en EF Core

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`Any()`/`AnyAsync()`** | `bool` | Determina si **al menos un** elemento satisface la condición. | `await db.Usuarios.AnyAsync(u => u.Rol == "Admin")` |
| **`All()`/`AllAsync()`** | `bool` | Determina si **todos** los elementos satisfacen la condición. | `db.Tareas.All(t => t.Prioridad > 0)` |
| **`Contains()`** | `bool` | Determina si una secuencia **contiene** un elemento específico. | `db.Categorias.Contains(miCategoria)` |

### Proyection y transformacion

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`Select()`** | `IQueryable<TResult>` | Transforma o proyecta la secuencia en un **nuevo formulario** (columnas/objeto). | `db.Autores.Select(a => a.NombreCompleto)` |
| **`SelectMany()`** | `IQueryable<TResult>` | Proyecta y **aplana** colecciones anidadas en una sola secuencia. | `db.Blogs.SelectMany(b => b.Posts)` |

### FIltrado

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`Where()`** | `IQueryable<T>` | Filtra la secuencia de valores basada en una **condición** (predicado). | `db.Libros.Where(l => l.Publicado > 2020)` |
| **`OfType<TResult>()`** | `IQueryable<TResult>` | Filtra elementos basados en un **tipo específico** (útil para herencia). | `db.Entidades.OfType<EntidadHija>()` |

### Ordenamiento

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`OrderBy()`/`OrderByDescending()`** | `IOrderedQueryable<T>` | Ordena los elementos en orden **ascendente** o **descendente**. | `db.Articulos.OrderBy(a => a.Fecha)` |
| **`ThenBy()`/`ThenByDescending()`** | `IOrderedQueryable<T>` | Define un **ordenamiento secundario** (debe seguir a `OrderBy`). | `db.Personas.OrderBy(p => p.Apellido).ThenBy(p => p.Nombre)` |

### Paginacion

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`Skip()`** | `IQueryable<T>` | **Omite** un número especificado de elementos al inicio. | `db.Comentarios.Skip(20)` |
| **`Take()`** | `IQueryable<T>` | Devuelve un número **especificado** de elementos al inicio. | `db.Productos.Take(10)` |

### Relaciones (Eager Loading)

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`Include()`** | `IQueryable<T>` | Incluye una **propiedad de navegación** (relación) en la consulta. | `db.Pedidos.Include(p => p.Cliente)` |
| **`ThenInclude()`** | `IIncludableQueryable<T, TProperty>` | Encadena la inclusión para cargar **relaciones anidadas** (más profundas). | `db.Pedidos.Include(p => p.Cliente).ThenInclude(c => c.Direccion)` |

### Modificadores de Consulta

| Método | Tipo de Retorno | Descripción Muy Corta | Ejemplo Aplicado (C#) |
| :--- | :--- | :--- | :--- |
| **`AsNoTracking()`** | `IQueryable<T>` | Indica al contexto **no rastrear** entidades (para consultas de solo lectura). | `db.Reportes.AsNoTracking().ToList()` |
| **`GroupBy()`** | `IQueryable<IGrouping<TKey, TElement>>` | **Agrupa** los elementos que comparten una clave en común. | `db.Ventas.GroupBy(v => v.Mes)` |
| **`Distinct()`** | `IQueryable<T>` | Retorna solo los elementos **únicos** o distintos. | `db.Correos.Select(c => c.Dominio).Distinct()` |


### guardar nuevo elemento




## Data Annotations en EF Core

| Atributo | Ejemplo | Descripción |
|-----------|----------|-------------|
| `[Key]` | `[Key] public int IdRol { get; set; }` | Indica que la propiedad es la **clave primaria (PK)**. |
| `[DatabaseGenerated(DatabaseGeneratedOption.Identity)]` | `[DatabaseGenerated(DatabaseGeneratedOption.Identity)]` | Hace que la columna sea **auto incremental**. |
| `[DatabaseGenerated(DatabaseGeneratedOption.Computed)]` | `[DatabaseGenerated(DatabaseGeneratedOption.Computed)]` | Valor **calculado por la BD** (ej. timestamp, fórmula). |
| `[Table("NombreTabla")]` | `[Table("Roles")]` | Define el **nombre de la tabla** en la base de datos. |
| `[Column("NombreColumna")]` | `[Column("DescripcionRol")]` | Define el **nombre de la columna** en la tabla. |
| `[Column(TypeName = "nvarchar(200)")]` | `[Column(TypeName = "decimal(18,2)")]` | Especifica el **tipo de dato SQL**. |
| `[Required]` | `[Required] public string Nombre { get; set; }` | Indica que el campo es **obligatorio** (`NOT NULL`). |
| `[MaxLength(100)]` | `[MaxLength(100)]` | Define el **tamaño máximo** de la cadena. |
| `[StringLength(100)]` | `[StringLength(100, MinimumLength = 3)]` | Similar a `MaxLength`, pero también puede definir **mínimo**. |
| `[MinLength(3)]` | `[MinLength(3)]` | Define la **longitud mínima** de la cadena. |
| `[Precision(10, 2)]` | `[Precision(10, 2)] public decimal Precio { get; set; }` | Define **precisión y escala** para valores numéricos o decimales. |
| `[ForeignKey("PropiedadNavegacion")]` | `[ForeignKey("Rol")] public int IdRol { get; set; }` | Define la **clave foránea (FK)** hacia otra entidad. |
| `[InverseProperty("PropiedadRelacionada")]` | `[InverseProperty("Usuarios")]` | Indica la **propiedad inversa** en una relación (útil en relaciones múltiples). |
| `[NotMapped]` | `[NotMapped] public string NombreCompleto => Nombre + " " + Apellido;` | Excluye la propiedad del mapeo a la base de datos. |
| `[ConcurrencyCheck]` | `[ConcurrencyCheck] public string Email { get; set; }` | Marca el campo para **control de concurrencia**. |
| `[Timestamp]` | `[Timestamp] public byte[] RowVersion { get; set; }` | Crea una **columna de versión binaria** para control de concurrencia. |
| `[Index("NombreIndice", IsUnique = true)]` | `[Index(nameof(Email), IsUnique = true)]` | Crea un **índice** (único o no). |
| `[Unicode(false)]` | `[Unicode(false)] public string Codigo { get; set; }` | Define si la columna es **Unicode o no** (`nvarchar` o `varchar`). |
| `[Comment("Texto de comentario")]` | `[Comment("Fecha de creación del registro")]` | Agrega un **comentario** a nivel de columna o tabla en la BD. |


