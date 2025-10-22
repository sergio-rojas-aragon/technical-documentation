---
title: Entity Framework Core
layout: home
parent: DotNet
nav_order: 2
---

# Entity Framework Core


## Instalacion

```
dotnet tool install --global dotnet-ef
```

## Migraciones

Cuando se utilizan las migraciones, si existen datos y se modifica las tablas, se trata de mantener la data adaptando las tablas modificadas. Si el cambio es muy grande se tiene que eliminar toda la Base de datos, ya que la migracion fallara al no poder realizar el cambio necesario en la informacion.

### Iniciar migraciones y tambien aplica para posteriores

Donde dice Incial es el nombre de la migracion, por lo que puede ser el nombre que uno quiera:

```
dotnet ef migrations add Inicial
```

### actualizar migraciones

```
dotnet ef database update
```

### listar migraciones

```
dotnet ef migrations list
```

### revertir migracion a un paso anterior

el nombre de las migraciones tiene fecha, es parte del nombre asi que se tiene que incluir
Esto puede fallar

```
dotnet ef database update nombreCompletoMigracion
```


### Eliminar migraciones

```
dotnet ef migrations remove
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


