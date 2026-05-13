## Cómo crear una clave primaria
- Crear una propiedad de solo lectura basada en un campo de respaldo estático para llevar la cuenta de objetos creados
- Permite hacer saber cuántos productos se han creado en total
- No es útil en entornos multithread porque varios hilos podrían leer su valor a la vez
```csharp
public class Producto
{
    private static int _contadorProductos = 0; //Campo estático: solo hay una copia compartida por todas las instancias de la clase

    public int Id { get; } // Esto no es multithread-safe
    public string Nombre { get; }

    public Producto(string nombre)
    {
        _contadorProductos++;
        Id = _contadorProductos;
        Nombre = nombre; // Se debería comprobar que nombre no sea ni null ni vacío
    }

    public static int ProductosCreados => _contadorProductos;  // Propiedad estática: permite acceder al contador sin un objeto

    public override string ToString() => $"ID: {Id}, Nombre: {Nombre}";
}

public class Program
{
    public static void Main()
    {
        Producto producto1 = new("Producto A");
        Console.WriteLine(producto1);
        Producto producto2 = new("Producto B");
        Console.WriteLine(producto2);
        Console.WriteLine($"Total de productos creados: {Producto.ProductosCreados}");
    }
}
```

- También se puede crear con un GUID
- Ventajas: Multithread-safe. Útil en sistemas distribuidos
- Desventajas: ocupa 16 *Bytes* frete a los 4 de un `int`. Es más difícil de leer para humanos
```csharp
public class Producto
{
    public Guid Id { get; } 
    public string Nombre { get; }

    public Producto(string nombre)
    {
        Id = Guid.NewGuid(); // Multithread-safe. Útil en sistemas distribuidos
        Nombre = nombre; // Se debería comprobar que nombre no sea ni null ni vacío
    }
    public override string ToString() => $"Id={Id}, Nombre={Nombre}]";
}

public class Program
{
    public static void Main()
    {
        Producto producto1 = new("Producto A");
        Console.WriteLine(producto1);
        Producto producto2 = new("Producto B");
        Console.WriteLine(producto2);
    }
}
```