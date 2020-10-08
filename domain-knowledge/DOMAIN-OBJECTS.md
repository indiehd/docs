# Domain Knowledge and Business Rules

## Terminology

### Service

The "Service" refers collectively to the software and accompanying third-party tools
that comprise the indieHD suite, _installed and "running" on a server_.

### Service Instance

A "Service Instance" refers to _one particular "installation"_ of the software
and accompanying third-party tools that comprise the indieHD suite.

### Service Operator

The "Service Operator" refers to the entity that operates the "Service". indieHD, LLC,
operates a "Service Instance", but is not necessarily the sole, exclusive "Service Operator". 

Given that the indieHD suite is licensed in such a way that any entity may setup a
"Service Instance" and act as the "Service Operator" thereof, a "Service Operator"
could therefore be anybody. For this reason, the terms "We" and "Us" are avoided
in favor of "Service Operator".

**IMPORTANT**: In cases in which indieHD, LLC is _not_ the "Service Operator",
there is no guarantee that any provision of this document is applicable or accurate,
as any entity may modify the "Service" in any way, without restriction, provided
such modifications do not violate the associated copyright and licensing provisions.

## Domain Objects ("Models")

### User

A `User` contains the bare-minimum information required for an entity to authenticate:
`username` (or `email`) and `password` (which is stored as a hash, and not
the literal password string).

#### Relationships

- `User` `hasOne()` `Account`
- `User` `hasMany()` entities (`CatalogEntities`)

#### Access Policy

- Anybody may create a `User`.
- Only the authenticated owner may view a `User`.
- Only the authenticated owner may update a `User`.
- Only the authenticated owner may delete a `User`.

### Account

An `Account` is a merely an extension of a `User`. Whereas a `User` contains the
bare-minimum information required for an entity to authenticate (email address
and password), an `Account` contains additional metadata.

This additional metadata includes fields such as name, address, and other
contact information that is **entirely optional** for the `User` to provide, but that
would be required under certain circumstances, such as if the account-holder were
to win a physical prize that requires shipping, such as a t-shirt or CD.

An `Account` cannot exist without a `User`, and vice versa.

**`Account` information is private.**

#### Relationships

- `Account` `belongsTo()` `User`
- `Account` `belongsTo()` `Country`

#### Access Policy

- Anybody may create an `Account`.
- An account-holder may view their own `Account`.
- An account-holder may update their own `Account`.
- An account-holder may soft-delete their own `Account`.

### CatalogEntity

In essence, a `CatalogEntity` is a person or company
that is _legally responsible_ for an `Artist` or `Label` whose original creative
works (music) are made available for public acquisition through the service. The
term "acquisition" is used in favor of "purchase" because `Artists` and `Labels`
may elect not to charge money for their works under certain circumstances.

The `CatalogEntity` concept is best illustrated by example:
 
- A traditional "label manager" might, for example, be the `CatalogEntity` that is
 associated with a given record label. Consequently, the address, phone number,
 and other information in the `CatalogEntity` object might be those of a
 corporate record label.
- By contrast, a single individual making music in a home studio and publishing
 it under a personal moniker might use his own name and home address when
 supplying the required `CatalogEntity` information.

When a `User` requests an `Artist` or `Label` `Profile`, they are asked, in effect, "Who
should we contact if we need to reach the `Artist` or `Label` in an official,
legal capacity, such as if the service owes unclaimed monies to the `Artist` or `Label` that you represent,
or if a legal dispute, e.g., in relation to a copyright claim, requires official
correspondence?" The contact information that the `User` provides in response is
stored as a `CatalogEntity`.

Stated another way, a `CatalogEntity` contains the information
required for the service operator to be able to transact with the `Artist` or `Label`
(e.g., pay money to the `Artist` or `Label`), and to carry-out its contractual
obligations under the service's Terms of Use.

**`CatalogEntity` information is private.**

#### Relationships

- `CatalogEntity` `morphTo()` `Catalogable`
- `CatalogEntity` `belongsTo()` `User`
- `CatalogEntity` `belongsTo()` approver (`User`)
- `CatalogEntity` `belongsTo()` deleter (`User`)
- `CatalogEntity` `belongsTo()` `Country`

#### Access Policy

- Anybody may create a `CatalogEntity` and the associated object (`Artist` or
  `Label`), but a `CatalogEntity` is inactive, by default, until a service
  operator staff member approves it, manually, at which time the `is_active`
  field is set to `true` and the `approver_id` field is set to the approving
  user's ID.
- A `CatalogEntity's` owning `User` may modify any of the object's informational
  fields: `first_name`, `last_name`, `company`, `title`, `email`, `address_one`,
  `address_two`, `city`, `territory`, `country_code`, `postal_code`, `phone`,
  `alt_phone`.
- A `CatalogEntity's` owning `User` may soft-delete the object, in which case
  the `deleter_id` field is set to the `User's` ID and the `deleted_at` field
  is set to the current time.
- A service operator staff member may set the `is_active` field to `false`, which
  renders the `CatalogEntity` inactive, thereby causing it and any associated
  `Artists` (in a `Label's` case) and `Albums` (in an `Artist's` case) to be
  inaccessible on all API endpoints. To be clear, if a `Label` is set to inactive,
  all of its associated `Artists` and all _their_ associated `Albums` are
  implicitly inactive.

### Artist

An `Artist` is an entity (the term "person[s]" is avoided here because it's
needlessly specific) who asserts authorship over an `Album`. An `Artist's` name
is essentially a "moniker"; it does not have to be a "real name", and it could
even be a phrase, such as _The Artist Formerly Known as Prince_, to cite a 
familiar example.

An `Artist` cannot exist without an associated `CatalogEntity`.

#### Relationships

- `Artist` `morphOne()` `Catalogable`
- `Artist` `morphOne()` `Profile`
- `Artist` `belongsTo()` `Label`
- `Artist` `hasMany()` `Song` through `Album`
- `Artist` `hasMany()` `Album`
- `Artist` `morphMany()` `Featured`

#### Access Policy

- A `Label` has the same access as any `Artist` under its control.
- Anybody may create an `Artist`, but it is effectively disabled until approved
  (see `CatalogEntity` access policy).
- The `User` associated with an `Artist's` owning `CatalogEntity` may set the
  `label_id` field on the `Artist` object by way of accepting a `Label's`
  invitation to join the `Label`.

### Label

A `Label` refers to what is known in music-industry parlance as a "record label".
While a complex entity in practice, a "record label", or `Label`, for the purposes
at hand, is a collection of `Artists`.

In a typical artist-label relationship, the `Label` exerts complete managerial
control over the `Artist`, and so the same is true here.

#### Relationships

- `Label` `morphOne()` `CatalogEntity` (`Artist` or `Label`)
- `Label` `morphOne()` `Profile` (`Artist` or `Label`)
- `Label` `hasMany()` `Artist`
- `Label` `hasMany[Album]Through[Artist]()`

#### Access Policy

- A `Label` is able to perform _any_ action that an `Artist` would normally be
  able to perform, for any `Artist` under its control.
- Due to the potential for conflicts of interest to arise between an `Artist` and a
  `Label` , were they not controlled by the same entity/account-holder, a given
  `Artist` that is associated with a `Label` cannot be controlled separately by
  an `Artist`. In other words, an `Artist` is either self-controlled, or controlled
  by a `Label` (but not both).

### Profile

`Artists` and `Labels` may, _optionally_, complete a `Profile`. The `Profile's`
only purpose is to increase search-engine visibility and educate the public
about the `Artist` or `Label`. `Profile` fields include city, country, official
website URL, etc. 

**`Profile` information is public.**

#### Relationships

- `Profile` `morphTo()` profilable (`Artist` or `Label`)
- `Profile` `belongsTo()` `Country`

#### Access Policy

- Any `Artist` or `Label` may create a `Profile`.
- Anybody may view a `Profile`.
- An `Artist` or `Label` may update its own `Profile`.
- An `Artist` or `Label` may force-delete its own `Profile`.
- When an `Artist` or `Label` is deleted, any associated `Profile` is likewise
  deleted.

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

- `Album` `hasMany()` `Song`
- `Album` `belongsTo()` `Artist`
- `Album` `belongsToMany()` `Genre`
- `Album` `belongsTo()` `Deleter (User)`
- `Album` `morphMany()` copies sold (`DigitalAsset`)

#### Access Policy

- A `Label` has the same access as any `Artist` under its control.
- An `Artist` may create an `Album`.
- Anybody may view an `Album` if it is active; if inactive, only the owning
  `Artist` may view it.
- An `Artist` may update their own `Album`, with certain restrictions: once _published_
  (i.e., made available to the public), the number of `Songs` and the order
  thereof cannot be changed, even if the `Artist` subsequently deactivates the `Album`.
- An `Artist` may force-delete their own `Album`, provided that no `Song` thereon
  has ever been purchased. If at least one `Song` thereon has been purchased,
  the `Artist` may soft-delete their own `Album`. If force-deleted, all associated
  `FlacFile` records, and their binary filesystem data, are also hard-deleted. In
  the case of soft-deletion, any `FlacFile` associated with a `Song` that has ever
  been purchased is preserved, and all others, along with their binary filesystem
  data, are hard-deleted. This policy ensures that customers who have purchased
  a `Song` are able to access it indefinitely.

#### Logic for Applying Full-Album Price Discount

The discount amount equals the difference between the full-album price and the
sum of the individual `Songs` on the `Album`.

A simple example is an `Album` that contains 10 `Songs`, each of which is
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

This discounting strategy is not specific to Microsoft Commerce Server; it is a
widely understood technique, but Microsoft's explanation is clear, concise,
and includes detailed examples.

### Song

A `Song` is exactly what it sounds like. An `Album` contains one or more `Songs`.

An `Artist` may disable a `Song` at any time, which removes it from public
visibility and prevents it from being added to a customer's shopping cart.
Disabling a `Song` has no effect for customers who have already purchased the
`Song`, and it will continue to appear in their "Purchased Music" history.
An `Artist` may want to disable a `Song` for any number of reasons,
ranging from simply not wanting it to be available anymore, to addressing a
copyright infringement claim that does not affect any other track on the `Album`
(in which case disabling or deleting the entire `Album` may not be necessary).

Each `Song` has its own price, which the owning `Artist` specifies.
The minimum `Song` price is $0.35, which is necessary to ensure that the payment
processing cost for a `Song` purchased individually does not exceed its price.
In other words, if a `Song` is priced at $0.05, for example, but the minimum payment
processing charge is 2.9% + $0.30 (this is a typical real-world rate),
payment processing would cost $0.31, thereby yielding a $0.26 loss for the service
operator and zero profit for the `Artist`.

The astute reader may be wondering why individual `Songs` cannot be priced
under $0.35 as long as they are purchased in quantities sufficient to offset the
payment processing cost. Technically, this is possible to implement, but in practice,
it leads to a confusing shopping experience because there can be no up-front
price listed for `Songs` under $0.35 (given that the discount can be calculated
only after `Songs` are added to the shopping cart). Even if customers are shown
the $0.35 price prior to adding the `Song` to the cart, and any applicable
discount is applied thereafter, the reason for the discount is confusing and
unintuitive. Accordingly, the authors have chosen not to pursue such a pricing
structure as yet.

#### Relationships

- `Song` `belongsTo()` `Album`
- `Song` `belongsTo()` `FlacFile`
- `Song` `morphMany()` copies sold (`DigitalAsset`) 
- `Song` `morphOne()` asset (`DigitalAsset`) 

#### Access Policy

- Any `Artist` may create a `Song`.
- Anybody may view a `Song` if its `is_active` flag is truthy, but only as long
  as the related `Album`'s `is_active` flag is also truthy.
- If a `Song`'s or its related `Album`'s `is_active` flag is falsey, only the
  owning `Artist`, or anybody who has purchased access to it, may view the `Song`.
- All rules that apply to the `is_active` flag apply equally to the
  soft-deletion (`deleted_at`) flag.
- Once the associated `Album` is activated, `Songs` cannot be added, deleted,
  or reordered. If the number or order of `Songs` on an `Album` must be changed,
  the `Album` can be deleted (if nobody has ever purchased it, whether in part
  or in full) or disabled (if anybody has ever purchased it, in part or in full).
- A `Song` cannot be deleted directly; a `Song` is deleted only when the
  associated `Album` is deleted. The rules that apply to `Album` deletion
  essentially "cascade" down to `Song` deletion, with regard to soft-deletion vs.
  force-deletion: if the `Album` is force-deleted, so are all associated `Songs`.
  If the `Album` is soft-deleted, any `Songs` that have ever been purchased are
  soft-deleted.

### FlacFile

A `FlacFile` represents an audio file encoded in the Free Lossless Audio Codec
(FLAC) format (or the metadata acquired therefrom, more accurately).

When an `Artist` uploads a lossless (that is, uncompressed or reversibly-compressed)
audio file, which is required, the service converts it to a FLAC file for long-term storage. Once
stored in FLAC format, the audio file can be converted to any other format, such
as Microsoft WAVE (WAV), MPEG-3 (MP3), Apple Lossless (ALAC), Advanced Audio Codec (AAC), etc.

The service favors the FLAC format for a number of reasons:

- FLAC is unencumbered by the many patents and royalty fees that plague competing formats.
- FLAC encoding and decoding tools are cost-free and open-source software.
- FLAC is technically superior to most, if not all, competing formats, offering
 the most features and flexibility.
 
When customers purchase music from the catalog, they are able to download their
purchased music in FLAC format, in addition to several other formats:

- WAV
- MP3
- AAC

#### Relationships

- `FlacFile` `hasMany()` `Song`

#### Access Policy

- Any `User` that is associated with a `CatalogEntity` whose `approved_at` field
  is non-null, and whose `deleted_at` field is null, may create a `FlacFile`.
- Anybody may read a `FlacFile` record from the _database_, but not necessarily
  the corresponding binary file from the filesystem.
- Only customers who have purchased access to a given `FlacFile` may retrieve
  the associated binary data from the filesystem.
- The `User` who is associated with a given `CatalogEntity` may delete a `FlacFile`
  that is under its control, but only under the following conditions:
  - The `FlacFile` is being overwritten and is not associated with any other `Song`.
    - If the `FlacFile` _is_ associated with another `Song`, the `User` is
      prompted: "The audio file that you're replacing is associated with other
      songs. Which of those songs, if any, should be updated with this audio file?"
      (The `User` is presented with a list, and appropriate UI controls, to
      select which `Songs'` relationships, if any, should be updated.) If the
      `User` selects every `Song`, the "orphaned" `FlacFile` is deleted, along
      with its binary filesystem data.
  - A `Song` is deleted that would cause the `FlacFile` to be "orphaned"; in such
    cases, the `FlacFile` is deleted, along with the associated binary filesystem
    data.

### Featured

A `Featured` `Artist` or `Label` is featured prominently within the digital
catalog, so that it may reach a broader audience.

#### Relationships

- `Featured` `morphTo()` featurable (`Artist` or `Label`)

#### Access Policy

- A scheduled task populates the `Featured` list, which is otherwise read-only.

#### Logic for Featuring Artists

To be featured, an `Artist` must meet the following criteria:

1. Must have an `Artist` `Profile`.
2. Must have at least one `Album` whose status is active (i.e., appears publicly).
3. Must have at least one `Song` on the aforementioned `Album` whose status is active.
4. Must not have been featured in the past 180 days.

To be featured, a `Label` must have at least one `Artist` that meets the above
criteria.

Featured `Artists` and `Labels` are selected at random and are featured for a
period of 7 days.

### Genre

One or more `Genres` may be assigned to an `Album`. Assigning one or
more `Genres` to an `Album` enables the `Album` to appear in search results for
the associated `Genre`, thereby making it easier for potential customers to find
music in their preferred `Genre`.

The service provides a pre-set list of `Genres` derived from the ID3v1 MP3 tag
specification. Additionally, `Artists` may request that new `Genres` are added,
which, upon request, are queued for administrative approval. This policy prevents
`Artists` from adding duplicate `Genres` (i.e., with alternate spellings) and
from using gratuitous profanity, inconsistent formatting, etc.

#### Relationships

- `Genre` `belongsToMany()` `Album`

#### Access Policy

- Anybody may read `Genres` whose `approved_at` field is non-null.
- Only service operator staff members may read `Genres` whose `approved_at` field
  is null.
- Any `CatalogEntity` may create a new `Genre`, _however_, a service operator
  staff member must approve the record before it is visible to anybody else.
  Upon approval, the `approved_at` field is set to the current date, and the
  `approver_id` is set to the `User` ID of the approving staff member.

### Country

A `Country` refers to the geographical meaning of the word and is used in
several domain areas. In some instances, a `Country` is a mere preference or superficial
label, but in other instances, it is an important indicator that dictates in which
countries the service is able to operate.

For example, a superficial use of `Country` is in the `User` `Profile`
context; the `User` can choose any `Country` they like, and its only purpose is to
inform others as to the `User's` country of origin or residence.

By contrast, payment processors are not able to operate in every country worldwide,
most commonly due to trade restrictions between countries,
which means that the service is not able to accommodate `CatalogEntities` residing
in certain countries. The service publishes a resource entitled _Countries That
We Are Able to Support_, which indicates in which countries its payment processor(s)
are able to operate. `CatalogEntities` residing in countries not on that list
are not permitted to offer their music through the service, per its Terms of Use.

Needless to say, the list of supported `Countries` depends entirely upon the
country in which the service operator resides. indieHD, LLC, resides within the
United States, but it is entirely possible that a different and unrelated service
operator installs the software and operates a service running thereon, in which
case the service might support an entirely different set of `Countries`.

#### Relationships

- `Country` `hasMany()` `Account`
- `Country` `hasMany()` `Profile`
- `Country` `hasMany()` `CatalogEntity`

#### Access Policy

- The `Country` list is static and therefore read-only.

### Order

An `Order` is a mechanism by which to associate `OrderItems`. Anybody &mdash;
even unauthenticated "guests" &mdash; may create an `Order` (i.e., by adding an
item to the shopping cart).

If the customer is not authenticated (not logged-in), a randomly-generated
token must be passed with each request that acts upon the `Order`. This token is
a "pseudo-random" UUID (v4) that is resistant to brute-force attacks and
unauthorized tampering.

#### Relationships

- `Order` `hasMany()` `OrderItem`
- `Order` `belongsTo()` `User`

#### Access Policy

- Anybody may create an `Order`.
- An authenticated `User` may view their own `Orders`, i.e., those whose
  `customer_id` field equals the `User` ID.
- Anybody may perform any action on an `Order` if they provide its token (UUID).

### OrderItem

An `OrderItem` is essentially a product that is added to a customer's shopping cart.
