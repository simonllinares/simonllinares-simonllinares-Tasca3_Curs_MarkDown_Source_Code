## Cómo hacer una Relación muchos a muchos
- Vamos a ver un ejemplo en el que un *Post* puede tener muchas *Etiquetas* y una *Etiqueta* puede estar asignada a muchos *Posts*
```csharp
public class Post
{
    public string CodigoHtml { get; set; }
    public List<Etiqueta> Etiquetas { get; } = [];

    public Post(string codigoHtml) => CodigoHtml = codigoHtml;

    public void AgregarEtiqueta(Etiqueta etiqueta)
    {
        Etiquetas.Add(etiqueta);
        etiqueta.Posts.Add(this);
    }

    public override string ToString() => CodigoHtml;

    public string PostYEtiquetas()
    {
        string resultado = $"Post: {CodigoHtml}\nEtiquetas:\n";
        foreach (var etiqueta in Etiquetas)
        {
            resultado += $"- {etiqueta}\n";
        }
        return resultado;
    }
}

public class Etiqueta
{
    public string TextoEtiqueta { get; set; }
    public List<Post> Posts { get; } = [];

    public Etiqueta(string textoEtiqueta) => TextoEtiqueta = textoEtiqueta;

    public override string ToString() => TextoEtiqueta;

    public string EtiquetaYPosts()
    {
        string resultado = $"Etiqueta: {TextoEtiqueta}\nPosts:\n";
        foreach (var post in Posts)
        {
            resultado += $"- {post}\n";
        }
        return resultado;
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Crear etiquetas
        Etiqueta etiquetaCSharp = new ("C#");
        Etiqueta etiquetaDotNet = new (".NET");
        Etiqueta etiquetaProgramacion = new ("Programación");
        List<Etiqueta> etiquetas = new List<Etiqueta> { etiquetaCSharp, etiquetaDotNet, etiquetaProgramacion };

        // Crear posts
        Post post1 = new Post ("Introducción a C#");
        Post post2 = new Post ("Desarrollo con .NET");
        List<Post> posts = new List<Post> { post1, post2 };

        // Asociar etiquetas a los posts
        post1.AgregarEtiqueta(etiquetaCSharp);
        post1.AgregarEtiqueta(etiquetaProgramacion);
        post2.AgregarEtiqueta(etiquetaDotNet);
        post2.AgregarEtiqueta(etiquetaProgramacion);

        // Mostrar resultados
        Console.WriteLine("Listado de posts y sus etiquetas:");
        foreach (var post in posts)
        {
            Console.WriteLine(post.PostYEtiquetas());
        }
        Console.WriteLine("Listado de etiquetas y sus posts:");
        foreach (var etiqueta in etiquetas)
        {
            Console.WriteLine(etiqueta.EtiquetaYPosts());
        }
    }
}
```