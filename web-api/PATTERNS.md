# Patterns we follow
In modern web development its important to follow good coding practices and adhere to specific
coding styles. This not only helps with the overall quality and integrity of your application, but
also helps with your (and other contributors) work flows by keeping everyone organized and on the
same page about whats expected.

**Design Patterns** [Wiki](https://en.wikipedia.org/wiki/Software_design_pattern#Practice)

**Coding Standards** [Wiki](https://en.wikipedia.org/wiki/Coding_conventions#Software_maintenance)

## PHP Standards Recommendation (PSR)
[Source](https://code.tutsplus.com/tutorials/psr-huh--net-29314)

If you're an avid PHP developer, it's quite likely that you've come across the abbreviation `PSR` 
which stands for `"PHP Standards Recommendation"`. The one we follow is `PSR-2`.

#### Why you should care (and participate)
PSR-2's purpose is to have a single style guide for PHP code that results in uniformly formatted 
shared code.

 * PSR-2 extends and expands PSR-1's basic coding standards. Its purpose is to have a single style 
 guide for PHP code that results in uniformly formatted shared code.

 * The coding style guide's rules were decided upon after 
 [an extensive survey](https://docs.google.com/spreadsheet/ccc?key=0AptAkq2qKI8adHBzQlZuVTlCSHVvQ2xJYUs3YWpqVVE) 
 given to the FIG voting members.

 * The rules in PSR-2, agreed upon by the voting members, do not necessarily reflect the preferences 
 of every PHP developer. Since the FIG's beginning, however, the PHP-FIG has stated that their 
 recommendations have always been for the FIG itself; others willing to adopt the recommendations 
 is a positive outcome.

The full [PSR-2 specification](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md) 
can be found in the [fig-standards](https://github.com/php-fig/fig-standards/) repository. 
Be sure to give it a read.

## S.O.L.I.D
[Source](https://stackify.com/solid-design-principles/)

SOLID is one of the most popular sets of design principles in object-oriented software development. 

It’s a mnemonic acronym for the following five design principles:

 * Single Responsibility Principle
 * Open/Closed Principle
 * Liskov Substitution Principle
 * Interface Segregation Principle
 * Dependency Inversion
 
All of them are broadly used and worth knowing.

## The Repository Pattern
[Source](http://shawnmc.cool/the-repository-pattern)

The Repository Pattern is one of the most popular patterns to create an enterprise level 
application. It restricts us to work directly with the data in the application and creates new 
layers for database operations, business logic, and the application’s UI. 

If an application ***DOES NOT*** follow the Repository Pattern, it may have the following problems:

 * Duplicate database operations codes
 * Need of UI to unit test database operations and business logic
 * Need of External dependencies to unit test business logic
 * Difficult to implement database caching, etc.
 
***Using*** the Repository Pattern has many advantages:

 * Your business logic can be unit tested without data access logic
 * The database access code can be reused
 * Your database access code is centrally managed so easy to implement any database access policies, 
 like caching
 * It’s easy to implement domain logic
 * Your domain entities or business entities are strongly typed with annotations and more

## Coding to an Interface
[Source](https://codeinphp.github.io/post/coding-to-interface/)

One of the nicest things you can add to your programming skills is coding to interface. 
One of the five principles of 
[S.O.L.I.D](https://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29) is 
[Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) 
which states:

    In object-oriented programming, the dependency inversion principle refers to a specific form of 
    decoupling software modules. When following this principle, the conventional dependency 
    relationships established from high-level, policy-setting modules to low-level, dependency 
    modules are reversed, thus rendering high-level modules independent of the low-level module 
    implementation details. The principle states:

        A. High-level modules should not depend on low-level modules. Both should depend on abstractions.

        B. Abstractions should not depend on details. Details should depend on abstractions.

#### Example
The Interface (aka Contract)
```php
interface DB
{
    public function connect($dsn, $user = '', $pass = '');
    public function query($query);
}
```

Here is `Mysql` class now implementing `DB`:
```php
class Mysql implements DB
{
    protected $db = null;

    public function connect($dsn, $user = '', $pass = '')
    {
        $this->db = new PDO($dsn, $user, $pass);
    }

    public function query($query)
    {
        return $this->db->query($query);
    }
}
```

Another database class implementing `DB`:
```php
class Sqlite implements DB
{
    ...
}
```

As you can see both `Mysql` and `Sqlite` implement methods imposed by `DB`. 

Finally here is our `User` class:
```php
class User
{
    private $database = null;

    public function __construct(DB $database)
    {
        $this->database = $database;
    }

    public function getUsers()
    {
        $users = $this->database->query('SELECT * FROM users ORDER BY id DESC');
        ...
    }
}
```
Notice that in constructor of above class, we are now passing `interface` rather than specific type of 
database.

Now we can use any database for the `User` class, here is example of `Mysql`:
```php
$database = new Mysql();
$database->connect('mysql:host=localhost;dbname=test', 'root', '');
$user = new User($database);
$user->getUsers();
```

And example of `Sqlite` database class:
```php
$database = new Sqlite();
$database->connect('sqlite:database.sqlite');
$user = new User($database);
$user->getUsers();
```