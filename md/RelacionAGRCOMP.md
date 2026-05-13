## Cómo hacer que una relación sea de agregación o de composición
### Agregación
En el apartado "Sólo navegación del lado uno al lado muchos" del punto "Cómo crear una relación uno a muchos" hemos visto el siguiente código:
```csharp
public class Blog
{
    public string CodigoHtml { get; set; }
    public List<Post> Posts { get; } // Desde el Blog puedo acceder a todos sus Posts

    public Blog(string codigoHtml)
    {
        CodigoHtml = codigoHtml;
        Posts = new List<Post>();
    }

    public void AgregarPost(Post post)
    {
        Posts.Add(post);
    }
}

public class Post
{
    public string CodigoHtml { get; set; }
    public Post(string codigoHtml) => CodigoHtml = codigoHtml;
}

public class Program
{
    public static void Main()
    {
        Blog miBlog = new Blog("Mi Blog");
        Post post1 = new ("Mi primer post");
        miBlog.AgregarPost(post1);
        Post post2 = new ("Mi segundo post");
        miBlog.AgregarPost(post2);

        Console.WriteLine("Contenido del Blog:");
        Console.WriteLine(miBlog.CodigoHtml);
        foreach (var post in miBlog.Posts)
        {
            Console.WriteLine("- " + post.CodigoHtml);
        }
    }
}
```
- Es importante fijarse cómo, en este ejemplo, el *Post* tiene una vida independiente del Blog, es decir, si un *Blog* desaparece, no tienen porqué desaparecer todos sus *Posts*.
- Por ejemplo, "Mi primer post" está guardado en `post1`. Si desaparece su blog: `miBlog`, el post continuará estando guardado en `post1`
- A este tipo de relación, se le llama **Agregación**.
### Composición
- En este tipo de relación queremos que al desaparecer un *Blog* desaparezcan junto con él todos sus *Posts*. Para conseguirlo, la única referencia a un *Post* debe estar guardada dentro del *Blog* y, por tanto, es necesario crear (`new`) el *Post* dentro del *Blog*. A este tipo de relación, se le llama **Composición** y a los métodos que crean objetos se les llama **Métodos factoría**
```csharp
public class Blog
{
    public string CodigoHtml { get; set; }
    public List<Post> Posts { get; } // Desde el Blog puedo acceder a todos sus Posts

    public Blog(string codigoHtml)
    {
        CodigoHtml = codigoHtml;
        Posts = new List<Post>();
    }

    public void AgregarPost(string codigoHtml)
    {
        Post nuevoPost = new (codigoHtml);
        Posts.Add(nuevoPost);
    }
}

public class Post
{
    public string CodigoHtml { get; set; }
    public Post(string codigoHtml) => CodigoHtml = codigoHtml;
}

public class Program
{
    public static void Main()
    {
        Blog miBlog = new Blog("Mi Blog");
        miBlog.AgregarPost("Mi primer post");
        miBlog.AgregarPost("Mi segundo post");

        Console.WriteLine("Contenido del Blog:");
        Console.WriteLine(miBlog.CodigoHtml);
        foreach (var post in miBlog.Posts)
        {
            Console.WriteLine("- " + post.CodigoHtml);
        }
    }
}
```