## Account Creation

Anybody may create an `Account`. The only required information is an email
address and password.

A `User` may close (reversible) or delete (permanent) an `Account` at any time,
subject to certain provisions that apply only to `Artists` and `Labels`. 

## Applying for an Artist Profile

Anybody may apply for an `Artist` profile. Doing so requires a basic `Account`,
and once logged-in, the `User` must complete an `Artist` profile application.

Service operator staff will review the application in a due-diligence capacity,
looking for obvious signs of fraud and incorrect or incomplete information.

The `User` will receive an email message upon approval and may begin creating
`Albums` immediately thereafter.

## Music Labels

The premise behind music `Labels` is to make it simple for "record labels" to
manage large music catalogs.

### Applying for a Label Profile

The process for applying for a `Label` profile is identical to that of applying
for an `Artist` profile.

### Adding a New Artist to a Label

A `Label` may, at any time, add a new `Artist` to reside under its management.
Doing so requires the `Label` to provide only the `Artist's` moniker
("stage name"), and the `Label` can begin adding `Albums` under that `Artist`
immediately.

In this scenario, the `Label` exercises complete control over the `Artist`, which
is in contrast to the next scenario, in which a `Label` invites an _existing_
`Artist` who is controlled by a separate entity (person or business) to come
under the `Label's` management.

When a `Label` creates an `Artist`, a `CatalogEntity` is not created for the
`Artist`, in which case the `Label's` `CatalogEntity` takes effect.

#### Use Case

When the `Artist` has no direct responsibility for managing its activities in
relation to the service, i.e., the `Label` manages the `Artist` completely, it
makes the most sense for the `Label` to create its `Artists` directly (rather
than having `Artists` create their own `Accounts` and then inviting them to join
the `Label`). This structure creates a lot less "paperwork" in that it eliminates
redundant data entry and streamlines the application and approval process for
each `Artist` on the `Label`.

This configuration is best suited for larger `Labels` that manage sizable music
catalogs and do not interact closely with their `Artists`.

### Inviting an Existing Artist to Join a Label

A `Label` may invite an existing `Artist` to join the `Label` at any time. Doing so only
requires the `Label` to enter the`Artist's` email address. The `Artist` will
receive an email message with further instructions.

Upon accepting the invitation, the `Artist` thereby grants the `Label` certain
managerial access to its `Account` and associated data.

In this scenario, a `Label` is able to perform the following actions on behalf
of an `Artist`:

- Manage `Albums`, including any existing albums (Create, Read, Update, Delete)
- Manage `Artist` `Profile` (Read, Update)
- Override `CatalogEntity` (that is, the `Label` cannot view or change this
  information; it can only override it, in which case its own `CatalogEntity`
  takes precedence); among other things, this controls which `CatalogEntity` is
  credited when a sale occurs

#### Use Case

When the `Artist` has a vested interest in its parent `Label's` activities in
relation to the service, the `Artist` may want to create and maintain its own
`Account`, which gives the `Artist` greater degrees of insight into, and control
over, its activities. In a sense, this configuration enables the `Artist` to
"keep the `Label` honest", because the `Artist` has real-time visibility into
sales data. In other words, it would be impossible for a `Label` to "rip-off"
an `Artist` under its control without the `Artist` knowing about it in this
configuration. Furthermore, even if the relationship between the `Artist` and
`Label` is terminated at any point, the `Artist` would retain its sales history
and therefore rank, which dictates its position within the larger music catalog.

This configuration is best suited for smaller `Labels` who have more intimate
relationships with their `Artists`.

### Additional Logic

- If a `Label` invites an existing `Artist`, the `Artist` must necessarily
  have a `CatalogEntity` with which it is associated, which the `Label` can
  override if desired (the Artist's associated `Account` and `User` are exempted
  specifically from `Label` access, which ensures that the `Label` cannot
  change the `Artist's` communication preferences to "cut them out of the loop").
- By default, a `Label's` `CatalogEntity` overrides the `Artist's`; accordingly,
  a `Label` must disable the override on a per-`Artist` basis to make the `Artist's`
  `CatalogEntity` effective.
- When a sale occurs, the *effective* `CatalogEntity`, at the moment of sale, is
  recorded and credited for the sale.
- A `Label` may release an `Artist` at any time, which simply breaks the
  association and revokes all access; both are notified of the change by email. 
- An `Artist` may leave a `Label` at any time, which simply breaks the
  association and revokes all access; both are notified of the change by email.

#### Effects of `CatalogEntity` Override

When the `Label` enforces this flag, the following conditions apply:

- Any earnings that are attributable to the `Artist` are paid to its `Label`.
  Conversely, if the `Label` does not enforce this flag, the `Artist` is paid
  directly.
- Any important correspondence demanding the `Artist's` attention, such as a
  copyright infringement claim (DMCA Take-Down Request), will be sent to the
  `Label`. Conversely, if the `Label` does not enforce this flag, the `Artist`
  receives any such correspondence directly. 

## Accounting Requirements for Recording Song and Album Sales

Due to the potential for an `Artist` to be associated with any number of different
`CatalogEntities` over its lifetime (though, only one at any given time),
keeping a detailed accounting of sales data for the indefinite benefit of both
the `Artist` and the one or more `Labels` requires the following to be recorded
at the time of each sale, along with the `Order` and its `OrderItems`:

- Artist ID
- _Effective_ `CatalogEntity` ID
  - For `Artists` that are not associated with a `Label`, this is the `Artist's`
    `CatalogEntity` ID.
  - For `Artists` that are associated with a `Label`, this is the `Label's`
    `CatalogEntity` ID if a) the `Artist` does not have its own `CatalogEntity`
    because the `Label` created the `Artist` directly; or b) the `Label` is
    enforcing the `CatalogEntity` override flag, which is the default setting
    when a `Label` invites an `Artist` to join it. Conversely, if the `Label`
    is not enforcing the `CatalogEntity` override flag for the `Artist`, then
    this is the `Artist's` `CatalogEntity` ID.

This enables the `Artist` to see which items it sold a) when it was not associated
with a `Label`, and b) when it was associated with each individual `Label`,
throughout the `Artist's` history.

Conversely, this enables the `Label` to see all of its historical sales that
are attributable to a given `Artist`, even once that `Artist` is no longer
associated with the `Label`.

## Adding a Song to an Album

Every `Song` must be associated with a `FlacFile`, and in fact, the `FlacFile`
may be associated with more than one `Song`. In effect, this means that the same
`FlacFile` can appear on more than one `Album`, the use-case for which is simple:
an `Artist` could very well upload a `Song` as a "single" (in which case it is
the only `Song` on the `Album`) and later include the same `Song` on a full-length
`Album`. Both "instances" of this `Song` can therefore share the same `FlacFile`,
but at the same time have different metadata attributes, such as price.

In the event that an `Artist` uploads the same audio file twice, a reference to
the existing file is created instead of storing the identical file again. As such,
if a `Song` is ever deleted, the binary audio file that belongs to the associated
`FlacFile` is deleted only if there are no remaining references to the `FlacFile`,
i.e., the `FlacFile` associated with the `Song` being deleted does not appear on
any of the `Artist's` other `Albums`.
 
This logic applies only a per-`Artist` basis; that is, if an `Artist` uploads
a file that is identical to a file that a _different_ `Artist` has uploaded
previously, then the identical copy _is_ stored. This rule ensures that `Artists`
have exclusive ownership over the audio files associated with their `Songs`, but
that needless duplication is avoided wherever practical. If it were
possible to share a `FlacFile` between two different `Artists`, seemingly simple
determinations, such as "Who uploaded this file?", become far more difficult,
with no real benefit beyond marginal storage gains.

## Account Closure

### For Non-Artist/Label

### For Artist/Label

## Purchasing Content

Purchasing content (music, etc.) is as simple as adding the content to the
shopping cart and proceeding through the checkout process.

When checking out, if the customer is not already logged into an `Account`, the
service provides the opportunity to do so.

If the customer elects not to create an `Account` (that is, they do not choose
a password), the customer checks-out as a "guest".

### As a Guest

Checking out as a "guest" is fastest and simplest, but it has one drawback: the
purchase is session-bound, so once the customer closes the browser, the
authorization to access the purchased content is revoked.

_However_, the service requires all customers to provide their email address
during checkout, and sends an Access Code via email, which allows the purchased
content to be retrieved at a later time.

#### Crediting an Account with a Past Purchase

Anyone with an `Account` may, at any time, credit the `Account` with a
past purchase by providing the Access Code that was sent via email upon making
the purchase. Upon redemption, the service behaves exactly as though the
customer was logged into an `Account` when the purchase was made.

Access Codes can be redeemed only once, and upon redemption,
the `access_code_redeemer` is set to the redeeming `User's` ID. Future attempts
to redeem the Access Code must evaluate this field for the `Order` in question
and prevent redemption. 

### As an Account-Holder

If the customer is logged into an `Account` during checkout, the `Account` is
credited automatically with the purchased content.

As such, the `User` can simply log into their `Account` at any time and see
their full purchase history, complete with each `Order` and the associated
`OrderItems`.

## Payments to Artists and Labels

## User Privacy

## Security
