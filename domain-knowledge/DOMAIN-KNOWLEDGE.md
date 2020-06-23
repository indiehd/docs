# Domain Knowledge and Business Rules

## Domain Objects ("Models")

### Account

An `Account` is a merely an extension of a `User`. Whereas a `User` contains the
bare-minimum information required for an entity to authenticate, an `Account`
contains more specific information. In particular, an `Account` contains the information
required for the service operator to be able to transact with the account-holder
(e.g., pay money to the account-holder, notify the account-holder of a claim against,
or in favor of, the account-holder as it relates to service-specific activities, etc.).

Limited-privilege `Users`, i.e., those whose activities cannot directly affect
the content made available through the service, are not required to create `Accounts`.
For example, customers who merely purchase music and choose to create a `User`
(that is, to specify a username and password) during the checkout process do not
create an `Account`.

In effect, the only `Users` who are required to create `Accounts` are `Artists`
and `Labels` (i.e., "Record Labels"). This requirement exists both because those
entities have the ability to affect content made available through the service,
and the service operator must be able to transact with them (e.g., to pay them
monies owed for goods or services sold on their behalf).

#### Relationships

- `Account` `belongsTo()` `User`
- `Account` `belongsTo()` `Country`

### Album

An `Album` is a collection of `Songs` with some additional metadata, such as
a title, production year, description, copyright information, etc.

Of this metadata, the "full album price" might be the most interesting. This is
the price that a customer pays to purchase all `Songs` on a given `Album` at
the same time. If specified, this price must necessarily be lower than the sum
of all the individual `Song` prices (as each `Song` also has its own price).

The `Artist` who owns an `Album` (not the service operator) sets its "full album
price", if desired. 

#### Relationships

- `Album` `hasMany()` `Songs`
- `Album` `belongsTo()` `Artist`
- `Album` `belongsToMany()` `Genres`
- `Album` `belongsTo()` `Deleter (User)`
- `Album` `morphMany()` `Copies Sold (OrderItem)`

#### Logic for Applying Full-Album Price Discount

The discount amount equals the difference between the full-album price and the
sum of the individual `Songs` on the `Album`.

A simple example would be an `Album` that contains 10 `Songs`, each of which is
priced at $1.00. If the `Artist` specifies a full-album price of $8.00, then the
discount amount equals $2.00.

When the customer adds the full album to the shopping cart, the $2.00 discount
is applied evenly over the 10 `Songs` in the shopping cart. As such, each `Song`
would be discounted in the amount of $2.00/10 = $0.20. However, this simple example
does not account for the potential for a fraction to result. For this reason, it
is necessary to employ a more complicated discounting strategy. This implementation
employs the "Distributing Percentage Off Discounts" strategy as described in
Microsoft's Commerce Server documentation:

https://docs.microsoft.com/en-us/previous-versions/commerce-server/ee785199(v=cs.20)?redirectedfrom=MSDN 

To be clear, this is not a "Microsoft-specific" strategy; it is widely understood,
and Microsoft's explanation is one of many (although, it is concise and among
the better examples).

### Artist
### CatalogEntity
### Country
### Featured
### FlacFile
### Genre
### Label
### Order
### OrderItem
### Profile
### Sku
### Song
### User
### UuidModel
