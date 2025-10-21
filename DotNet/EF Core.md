---
title: Entity Framework Core
layout: home
parent: DotNet
nav_order: 2
---

# Entity Framework Core

## Data Annotations en EF Core

| Atributo | Espacio de nombres | Descripción | Ejemplo |
|-----------|--------------------|--------------|----------|
| `[Key]` | `System.ComponentModel.DataAnnotations` | Indica que la propiedad es la **clave primaria (PK)**. | `[Key] public int IdRol { get; set; }` |
| `[DatabaseGenerated(DatabaseGeneratedOption.Identity)]` | `System.ComponentModel.DataAnnotations.Schema` | Hace que la columna sea **auto incremental**. | `[DatabaseGenerated(DatabaseGeneratedOption.Identity)]` |
| `[DatabaseGenerated(DatabaseGeneratedOption.Computed)]` | `System.ComponentModel.DataAnnotations.Schema` | Valor **calculado por la BD** (ej. timestamp, fórmula). | `[DatabaseGenerated(DatabaseGeneratedOption.Computed)]` |
| `[Table("NombreTabla")]` | `System.ComponentModel.DataAnnotations.Schema` | Define el **nombre de la tabla** en la base de datos. | `[Table("Roles")]` |
| `[Column("NombreColumna")]` | `System.ComponentModel.DataAnnotations.Schema` | Define el **nombre de la columna** en la tabla. | `[Column("DescripcionRol")]` |
| `[Column(TypeName = "nvarchar(200)")]` | `System.ComponentModel.DataAnnotations.Schema` | Especifica el **tipo de dato SQL**. | `[Column(TypeName = "decimal(18,2)")]` |
| `[Required]` | `System.ComponentModel.DataAnnotations` | Indica que el campo es **obligatorio** (`NOT NULL`). | `[Required] public string Nombre { get; set; }` |
| `[MaxLength(100)]` | `System.ComponentModel.DataAnnotations` | Define el **tamaño máximo** de la cadena. | `[MaxLength(100)]` |
| `[StringLength(100)]` | `System.ComponentModel.DataAnnotations` | Similar a `MaxLength`, pero también puede definir **mínimo**. | `[StringLength(100, MinimumLength = 3)]` |
| `[MinLength(3)]` | `System.ComponentModel.DataAnnotations` | Define la **longitud mínima** de la cadena. | `[MinLength(3)]` |
| `[Precision(10, 2)]` | `System.ComponentModel.DataAnnotations` (desde EF Core 6) | Define **precisión y escala** para valores numéricos o decimales. | `[Precision(10, 2)] public decimal Precio { get; set; }` |
| `[ForeignKey("PropiedadNavegacion")]` | `System.ComponentModel.DataAnnotations.Schema` | Define la **clave foránea (FK)** hacia otra entidad. | `[ForeignKey("Rol")] public int IdRol { get; set; }` |
| `[InverseProperty("PropiedadRelacionada")]` | `System.ComponentModel.DataAnnotations.Schema` | Indica la **propiedad inversa** en una relación (útil en relaciones múltiples). | `[InverseProperty("Usuarios")]` |
| `[NotMapped]` | `System.ComponentModel.DataAnnotations.Schema` | Excluye la propiedad del mapeo a la base de datos. | `[NotMapped] public string NombreCompleto => Nombre + " " + Apellido;` |
| `[ConcurrencyCheck]` | `System.ComponentModel.DataAnnotations` | Marca el campo para **control de concurrencia**. | `[ConcurrencyCheck] public string Email { get; set; }` |
| `[Timestamp]` | `System.ComponentModel.DataAnnotations` | Crea una **columna de versión binaria** para control de concurrencia. | `[Timestamp] public byte[] RowVersion { get; set; }` |
| `[Index("NombreIndice", IsUnique = true)]` | `Microsoft.EntityFrameworkCore` (desde EF Core 5) | Crea un **índice** (único o no). | `[Index(nameof(Email), IsUnique = true)]` |
| `[Unicode(false)]` | `Microsoft.EntityFrameworkCore` (desde EF Core 6) | Define si la columna es **Unicode o no** (`nvarchar` o `varchar`). | `[Unicode(false)] public string Codigo { get; set; }` |
| `[Comment("Texto de comentario")]` | `Microsoft.EntityFrameworkCore` (desde EF Core 5) | Agrega un **comentario** a nivel de columna o tabla en la BD. | `[Comment("Fecha de creación del registro")]` |

