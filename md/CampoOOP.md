## Cómo hacer que un campo sea obligatorio-opcional
- Campo obligatorio: no es nulable y se asigna en el constructor. Si el campo es un tipo referencia, en el constructor se comprobará que no es ni vacío ni `null`
- Campo opcional: es nulable y se inicializa a null
```csharp
public class Persona
{
    public string Nombre { get; } // Obligatorio
    public string DNI { get; } // Obligatorio
    public DateOnly? FechaNacimiento { get; set; } // Opcional. Permite fechas de nacimiento futuras!!

    public int Edad => FechaNacimiento.HasValue ? DateTime.Now.Year - FechaNacimiento.Value.Year : 0;

    public Persona(string nombre, string dni)
    {
        if (string.IsNullOrEmpty(nombre))
        {
            throw new ArgumentException("El nombre no puede estar vacío.", nameof(nombre));
        }
        Nombre = nombre;
        if (string.IsNullOrEmpty(dni))
        {
            throw new ArgumentException("El DNI no puede estar vacío.", nameof(DNI));
        }
        DNI = dni;
        FechaNacimiento = null;
    }

    private string DescripcionEdad() => FechaNacimiento.HasValue ? $"{Edad} años" : "Edad no disponible";
    public override string ToString() => $"{Nombre} ({DNI}) - Edad a fecha ({DateTime.Now.ToString("d/M/yy")}): {DescripcionEdad()}";
}

public class Program
{
    public static void Main()
    {
        Persona persona1 = new Persona("Juan Pérez", "12345678A");
        persona1.FechaNacimiento = new DateOnly(1990, 1, 1);
        Console.WriteLine(persona1);

        Persona persona2 = new Persona("María López", "87654321B");
        Console.WriteLine(persona2);
    }
}
```