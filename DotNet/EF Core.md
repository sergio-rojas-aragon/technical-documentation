---
title: Entity Framework Core
layout: home
parent: DotNet
nav_order: 2
---

# Entity Framework Core

## Data Annotations en EF Core

# Data Annotations en EF Core

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


