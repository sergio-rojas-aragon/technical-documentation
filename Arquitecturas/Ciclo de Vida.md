---
title: Ciclo de Vida
layout: home
parent: Arquitecturas
nav_order: 3
---

# Ciclo de vida de un proyecto C#
{: .no_toc }

{: .no_toc .text-delta }

1. TOC
{:toc}

## que es el ciclo de vida de un objeto en .Net

Cuando creas un objeto en .NET (por ejemplo, new `Circulo()`), el runtime:

1. Reserva memoria en el heap administrado.
1. Inicializa el objeto (constructor).
1. El objeto vive mientras haya referencias activas a él.
1. Cuando ya no hay referencias, el Garbage Collector (GC) lo marca para eliminar.
1. Finalmente, el GC libera la memoria y, si corresponde, ejecuta el finalizador.

Todo esto sucede automáticamente; tú no liberas memoria manualmente (a diferencia de C++).

## Generaciones del Garbage Collector (Generational GC)

Los objetos se agrupan en generaciones, dependiendo de su “edad” (cuánto tiempo llevan vivos):

| Generación | Qué representa                                                 | Características                           |
| ---------- | -------------------------------------------------------------- | ----------------------------------------- |
| **Gen 0**  | Objetos recién creados.                                        | Frecuentemente recolectados. Corto plazo. |
| **Gen 1**  | Objetos que sobrevivieron una recolección.                     | Intermedia.                               |
| **Gen 2**  | Objetos de larga vida (por ejemplo, objetos globales, cachés). | Recolectados con poca frecuencia.         |

**Ciclo:**

1. Creas un objeto → va a Gen 0.
2. Si sobrevive a una recolección → pasa a Gen 1.
3. Si sigue existiendo después → pasa a Gen 2.

Eventualmente, el GC limpia las generaciones cuando hay presión de memoria.

## Destructores



Es un método especial que se ejecuta antes de que el objeto sea destruido por el GC.

{: .highlight }
No se puede llamar a los finalizadores. Se invocan automáticamente.


Notacion: `~NombreDeClase()`

**Caracteristicas**

* No se llama manualmente (lo hace el GC).
* No sabes cuándo se ejecutará.

```
class First
{
    ~First()
    {
        System.Diagnostics.Trace.WriteLine("First's finalizer is called.");
    }
}

class Second : First
{
    ~Second()
    {
        System.Diagnostics.Trace.WriteLine("Second's finalizer is called.");
    }
}

class Third : Second
{
    ~Third()
    {
        System.Diagnostics.Trace.WriteLine("Third's finalizer is called.");
    }
}

/* 
Test with code like the following:
    Third t = new Third();
    t = null;

When objects are finalized, the output would be:
Third's finalizer is called.
Second's finalizer is called.
First's finalizer is called.
*/
```



## Dispose y Finalize (Patrón de limpieza de recursos)

.NET separa los recursos en dos tipos, Administrados y No administrados. Los no administrados se tienen que liberar manualmente

Ejemplo de como liberar recursos manualmente:

```
// se llama a la interface IDisposable
public class Archivo : IDisposable
{

    // Método Dispose
    public void Dispose()
    {
        stream.Close();
        stream.Dispose();
        Console.WriteLine("Archivo cerrado (Dispose)");
    }
}
```

y con using se ejecuta automaticamente:

```
using (var archivo = new Archivo("datos.txt"))
{
    archivo.Escribir("Hola mundo");
}
// Aquí se llama automáticamente a Dispose()
```

## Patrón combinado: Dispose + Finalize (cuando hay recursos no administrados)

Si tu clase administra recursos nativos, lo recomendable es usar ambos métodos, siguiendo el patrón oficial de Microsoft:

```
// llama a la interfaz
public class FiguraConRecursos : IDisposable
{
    private IntPtr recursoNativo; // Ejemplo de recurso no administrado
    private bool disposed = false;

    public FiguraConRecursos()
    {
        recursoNativo = /* obtener recurso nativo */;
    }

    // Finalizador
    ~FiguraConRecursos()
    {
        Dispose(false);
    }

    public void Dispose()
    {
        Dispose(true);

        // Evita que el GC vuelva a llamar a Finalize()
        GC.SuppressFinalize(this); 
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // Liberar recursos administrados aquí (objetos .NET)
            }

            // Liberar recursos no administrados aquí
            recursoNativo = IntPtr.Zero;
            disposed = true;
        }
    }
}

```

Ejemplo funcional en el repo https://github.com/sergio-rojas-aragon/ConceptosBasePOO

