## Cómo crear una relación uno a muchos
Vamos a trabajar el ejemplo de un *Blog* que puede contener cero o muchos *Post*
### Sólo navegación del lado uno al lado muchos
```csharp
// Clase principal (padre)
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

// Clase dependiente (hija)
// Desde un Post no puedo acceder al Blog
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
- Con esta estructura solo el *Blog* conoce a sus *Post* por lo que hay una mejor encapsulación
- Si hacemos una búsqueda de *Posts* que contienen una palabra clave, será más complicado obtener los *Blogs* en los que se publicaron dichos *Posts*
### Sólo navegación desde el lado muchos al lado uno
```csharp
public class Blog
{
    public string CodigoHtml { get; set; }

    public Blog(string codigoHtml) => CodigoHtml = codigoHtml;
}

public class Post
{
    public string CodigoHtml { get; set; }
    public Blog Blog { get; } // Desde un Post se puede acceder al Blog asociado

    public Post(string codigoHtml, Blog blog)
    {
        CodigoHtml = codigoHtml;
        Blog = blog;
    }
}

public class Program
{
    public static void Main()
    {
        Blog miBlog1 = new Blog("Mi Blog");
        Blog miBlog2 = new Blog("Otro Blog");

        List<Post> posts = [];

        Post post = new Post("Post 1", miBlog1);
        posts.Add(post);
        post = new Post("Post 2", miBlog1);
        posts.Add(post);
        post = new Post("Post 3", miBlog2);
        posts.Add(post);
        post = new Post("Post 4", miBlog1);
        posts.Add(post);

        foreach (Post p in posts)
        {
            Console.WriteLine($"Post: {p.CodigoHtml}, pertenece al Blog: {p.Blog.CodigoHtml}");
        }
    }
}
```
- El *Post* conoce a su *Blog*, pero este no conoce a sus *Posts*.
### Navegación bidireccional
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
        post.Blog = this; // Asignar el Blog al Post
        Posts.Add(post);
    }

    public override string ToString() => $"Blog: {CodigoHtml}";
}

public class Post
{
    public string CodigoHtml { get; set; }
    public Blog? Blog { get; set; } // Desde un Post se puede acceder al Blog asociado
    // Al crear un Post, todavía no está asignado a ningún Blog

    public Post(string codigoHtml) => CodigoHtml = codigoHtml;

    public override string ToString() => $"Post: {CodigoHtml}";
}

public class Program
{
    public static void Main()
    {
        Blog miBlog1 = new Blog("Mi Blog");
        Blog miBlog2 = new Blog("Otro Blog");

        List<Post> posts = [];

        Post post = new Post("Post 1");
        posts.Add(post);
        miBlog1.AgregarPost(post);
        post = new Post("Post 2");
        posts.Add(post);
        miBlog1.AgregarPost(post);
        post = new Post("Post 3");
        posts.Add(post);
        miBlog2.AgregarPost(post);
        post = new Post("Post 4");
        posts.Add(post);
        miBlog2.AgregarPost(post);

        Console.WriteLine("Partiendo de un Blog puedo obtener sus Posts:");
        Console.WriteLine(miBlog1);
        foreach (Post p in miBlog1.Posts)
        {
            Console.WriteLine("\t" + p);
        }

        Console.WriteLine("Partiendo de un Post puedo obtener su Blog:");
        foreach (Post p in posts)
        {
            Console.Write(p + " pertenece a ");
            Console.WriteLine(p.Blog);
        }
    }
}
```
- Esta opción tiene más riesgo de inconsistencias porque la información está duplicada
- Requiere mantener la sincronía de manera manual a la hora de crear, eliminar o cambiar *Blogs* y *Posts*