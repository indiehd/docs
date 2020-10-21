### Product

A `Product` is self-explanatory and consists of metadata such as `name`,
`description`, `price`, etc.

#### Relationships

- `Product` `hasMany()` `ProductImages`
- `Product` `belongsToMany()` `Attributes`
- `Product` `belongsToMany()` `Orders`

#### Access Policy

- Any `Artist` may create a `Product` (via creating a `Song`).
- Anybody may view any `Product`.
- Only the owning `Artist` may update a `Product`.
- Only the owning `Artist` may delete a `Product` (via deleting a `Song`).

### Cart

Much like a real shopping cart, a `Cart` is merely the vehicle by which
`Products` are added to an `Order`, i.e., "the cart carries the products to the
checkout counter, at which time the products are added to an order."

#### Relationships

- *None*

#### Access Policy

- Anyone may create a `Cart`.
- Anybody may perform any action on a `Cart` if they provide its identifier
  (UUID). This identifier is a "pseudo-random" UUID (v4) that is resistant to
  brute-force attacks and unauthorized tampering, and is generated when the
  `Cart` is created.

### Order

An `Order` is a mechanism by which to collate `Products`.

#### Relationships

- `Order` `belongsToMany()` `Products`

#### Pivots

##### `OrderProduct`

An `OrderProduct` differs from a `Cart` item in that the `Cart` item is
ephemeral and short-lived, whereas an `OrderProduct` is a "permanent record".

`Carts` are subject to "garbage collection" after a period of time, and are
coupled to a shopping cart API that may change over time, so they are not an
appropriate long-term storage mechanism for the items that a customer purchases.
For this reason, a `Cart` item is "converted" to an `OrderProduct` upon placing
an `Order` (i.e., checking-out successfully), at which time the item's price,
description, quantity, etc. are effectively "copied" from the `Cart` item to the
`OrderProduct` for long-term storage.  (Technically, the price, description, and
other metadata are actually copied from the `Product`, and not from the `Cart`
item directly, as the `Product` is the official "source of truth".)

#### Access Policy

- Anybody may create an `Order`.
- An authenticated `User` may view their own `Orders`, i.e., those whose
  `customer_id` field equals the `User` ID.
- Anybody may perform any action on an `Order` if they provide its identifier
  (UUID). This identifier is a "pseudo-random" UUID (v4) that is resistant to
  brute-force attacks and unauthorized tampering, and is generated when the
  `Order` is created.
- An `Order` cannot be deleted.

### Payment

A `Payment` constitutes financial reconciliation for an `Order`. Consequently, a
`Payment` cannot exist without a corresponding `Order`.

In a "digital-only" store, in which all transactions require "payment due upon
receipt", an `Order` is meaningless without a corresponding `Payment`. As such,
an `Order` and a `Payment` are created in a single database transaction in this
particular implementation; the `Order` first, and then the related `Payment`,
so, an `Order's` initial status will always be "paid" or equivalent.

#### Relationships

- `Payment` `belongsTo()` `Order`

#### Access Policy

- Anybody may create a `Payment`.
- The ability to view a `Payment` is granted via the associated `Order`.
- A `Payment` cannot be updated.
- A `Payment` cannot be deleted.
