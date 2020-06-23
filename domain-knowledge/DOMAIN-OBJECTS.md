# Domain Knowledge and Business Rules

## Terminology

### "Service"

The "Service" refers collectively to the software and accompanying third-party tools
that comprise the indieHD suite, _installed and "running" on a server_.

### "Service Instance"

A "Service Instance" refers to _one particular "installation"_ of the software
and accompanying third-party tools that comprise the indieHD suite.

### "Service Operator"

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

### Account

An `Account` is a merely an extension of a `User`. Whereas a `User` contains the
bare-minimum information required for an entity to authenticate (email address
and password), an `Account` contains additional metadata.

This additional metadata includes fields such as name, address, and other
contact information that is **entirely optional** for the `User` to provide, but that
would be required under certain circumstances, such as if the account-holder were
to win a physical prize that requires shipping, such as a t-shirt or CD.

An `Account` cannot exist without a `User`, and vice versa.

#### Relationships

- `Account` `belongsTo()` `User`
- `Account` `belongsTo()` `Country`

#### Access Policy

- Anybody may create an `Account`.
- An account-holder may view their own `Account`.
- An account-holder may update their own `Account`.
- An account-holder may soft-delete their own `Account`.

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

#### Access Policy

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

To be clear, this is not a "Microsoft-specific" strategy; it is widely understood,
and Microsoft's explanation is one of many (although, it is concise and among
the better examples).

### Artist

An `Artist` is an entity (the term "person[s]" is avoided here because it's
needlessly specific) who asserts authorship over an `Album`. An `Artist's` name
is essentially a "moniker"; it does not have to be a "real name", and it could
even be a phrase, such as _The Artist Formerly Known as Prince_, to cite a 
familiar example.

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
correspondence?" Very simply, the `User's` response describes the `CatalogEntity`.

Stated another way, a `CatalogEntity` contains the information
required for the service operator to be able to transact with the `Artist` or `Label`
(e.g., pay money to the `Artist` or `Label`), and to carry-out its contractual
obligations under the service's Terms of Use.

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

### Featured

A `Featured` `Artist` is featured prominently within the digital catalog, so that
it may reach a broader audience.

#### Logic for Featuring Artists

To be featured, an `Artist` must meet the following criteria:

1. Must have an `Artist` `Profile`.
2. Must have at least one `Album` whose status is active (i.e., appears publicly).
3. Must have at least one `Song` on the aforementioned `Album` whose status is active.
4. Must not have been featured in the past 180 days.

Featured `Artists` are selected at random and are featured for a period of 7 days.

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

### Label
### Order
### OrderItem
### Profile
### Sku
### Song
### User
### UuidModel
