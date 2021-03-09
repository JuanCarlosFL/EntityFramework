# EntityFramework

En el Command Prompt para crear una solución en blanco con el nombre que le demos a la carpeta escribimos

```
mkdir nombreDelProyecto
cd nombreDelProyecto
dotnet new sln
```

Esto nos crea una solución en blanco con el nombre que le hemos dado a la carpeta

Ahora creamos una carpeta que contendrá un proyecto de consola y abrimos Visual Studio Code a nivel de la solución

```
mkdir readData
cd readData
dotnet new console
cd ..
code .
```

Entity Framework busca clases que se convertirán en entidades y para ello usa AsNoTracking() y las clases deben tener DbSet, el flujo para la clase person sería:

db.Person.AsNotracking() coge de la clase DbSet<Person> --> Usa Linq y lo convierte a SQL --> SELECT Person.id, Name --> Modelo Relacial SQL --> foreach(...person)

- Creamos la clase Person

```C#
using System;

namespace readData
{
    public class Person
    {
        public int PersonId { get; set; }

        public string Name { get; set; }

        public string Surname { get; set; }

        public DateTime DOB { get; set; }
        
    }
}
```

Ahora viene la parte más importante y es crear una clase que tendrá la configuración de conexión con la base de datos y para ello debe heredar de DbContext,
El DbContext es una parte integral de Entity Framework.  Una instancia de DbContext representa una sesión con la BBDD.
Un DbSet dentro de un DbContext representa una tabla o una vista de la BBDD.
En la clase creamos la cadena de conexión, usamos el método OnConfiguring para hacer la conexión y añadimos el DbSet de qué queremos consumir

```C#
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.SqlServer;

namespace readData
{
    public class AplicationContext : DbContext
    {
        private const string connectionString = @"Data Source=localhost;Initial Catalog=TestPersons;Integrated Security=False;User Id=sa;Password=(YourPassword)";

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder){
            optionsBuilder.UseSqlServer(connectionString);
        }

        public DbSet<Person> Person {get; set;}
    }
}
```

Suponiendo que tenemos una BBDD con esta tabla y estos datos





