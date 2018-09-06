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

## Airbnb Coding Style

Airbnb has one of the most popular JavaScript style guides on the internet. It covers nearly every 
aspect of JavaScript as well.

You can view [Airbnb’s style guide](https://github.com/airbnb/javascript) on GitHub.