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

## Account Closure & Deletion

`Account` closure is implemented with "soft-deletion" on the `User` object, so it
is completely reversible. When a `User` closes an `Account`, all publicly
accessible content associated with that user disappears from the service
immediately. 

To reopen the account, the `User` may simply log in again and confirm the intention
to reopen the account.

Account deletion, on the other hand, is implemented with "hard-deletion", so it
is permanent and cannot be undone. 

### For Non-Artist/Label

When a `User` that is not associated with an `Artist` or `Label`, the following
objects are deleted *inside a transaction* (and in this order, to prevent
foreign key failures):

- `OrderItem`
- `Order`
- `Account`
- `User`

### For Artist/Label

#### For Artist

When a `User` is associated with an `Artist`, additional provisions apply when
deleting an `Account`.

When an `Artist` requests that its `Account` be deleted, the deletion is queued,
pending an eligibility check is against the following rules:

1. All albums have been disabled (this prevents customers from making further
   purchases, which would extend the deletion timeline for 90 more days).
2. All monies owed to the `Artist` have been paid to its associated `CatalogEntity`.
3. 90 days have elapsed since a customer last purchased the `Artist's` content.
4. All of the `Artist's` albums have been deleted.
5. All of the `Artist's` uploaded images have been deleted.

Once every 24 hours, eligibility is evaluated, and if any of those statements is
false, the `Account` is ineligible for deletion, and the service will complete
any steps that it can, automatically, before the next evaluation.

Eventually, all statements will evaluate as true, at which time the `Account`,
and all associated content (database records and files) is hard-deleted.

Upon successful deletion, the following objects are deleted, *inside a transaction*
(and in this order, to prevent foreign key failures):

- `OrderItem`
- `Order`
- `Profile`
- `Account`
- `User`

*Before the transaction is committed*, a confirmation email message is queued to
be sent to the email address associated with the `User` record. Upon successful
queueing, the transaction is committed.

#### For Label

When a `User` is associated with a `Label`, the `Account` deletion process is
exactly the same as it is for an `Artist`, except that the eligibility rules are
evaluated against all of the individual `Artists` associated with the `Label`,
"in a loop".

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

### Access to Purchased Content

Access to purchased content is guaranteed for a period of 90 days from the date
of purchase. Beyond that period, it is possible that an `Artist` whose content
a customer has purchased deletes that content from the service, in which case
it is no longer accessible. Provided an `Artist` does not delete any of its content,
customers will have access to it indefinitely.

As such, customers are encouraged to download purchased content immediately
and not rely on the service as a permanent archive of their purchases.

## Payments to Artists and Labels

### Fees

First and foremost, it is important to understand that all payment processors
charge some type of fee for accepting online payments from customers. That fee
is passed-on to the `Artist` or `Label`. For customers in the United States,
that fee is 2.9% + $0.30. So, on a $10.00 USD sale, an `Artist` would incur a
$0.59 USD fee. For customers outside the United States, there is an additional
3.0% charge for any currency conversion and a 1.5% international transaction fee
to receive payments from another country.

In addition, the service charges its own fee of 10%, with the sole purpose of
covering its operational expenses. Given the example above, that fee would be
$1.00 USD. In total, the `Artist` would "net" $10.00 USD - $1.59 USD = $8.41 USD,
which is 84.1% of the order total.

Finally, the service operator's payment processor charges a fee to pay out an
`Artist` or `Label`. The exact fees associated with `Artist` and `Label` payouts
are variable and depend upon the transaction amount, the `Country` in which
the `Artist's` or `Label's` `CatalogEntity` resides, and the target currency.
In order to minimize the payout fees incurred, the service leverages a variety
of PayPal APIs to send payments to `Artists` and `Labels`.

For payments under $10.00 USD, the Micropayments API is used. For `CatalogEntities` inside the
United States, the fee is 5.0% of the transaction amount, plus a fixed fee based
on the currency (USD is $0.05, for example). For `CatalogEntities` outside the
United States, the fee is 6.5%, plus the same fixed fee based on the currency.

For payments $10.00 USD and over, the Payouts API is used. For `CatalogEntities`
inside the United States, the payment processor charges a flat fee of $0.25 USD,
whereas outside the U.S., the fee is 2.0% of the payout amount, up to a maximum
of $20.00 USD (or target currency equivalent).

The payment processor's fee schedule is complicated, by nature, and the fact that an `Order` can
contain more than one `Artist's` `Songs` compounds the complexity even further,
because the fees must be allocated to each `Artist` in proportion to the
`OrderItems` from which the `Order` is composed.

For example, if an `Order` contains two different `Artist's` `Songs`, and
contains 10 `Songs` in total, 5 of which are associated with each `Artist`,
and each `Song` is priced at $1.00 USD, the two `Artists` would share the fees
equally, i.e., "split the fees down the middle". However, 8 of the 10 `Songs`
belong to one `Artist`, that `Artist` would incur 80% of all associated fees,
and the other `Artist` 20%. Of course, in reality, the `Songs` may not be priced
the same, in which case the computations are even less straightforward.

At any given time, an `Artist` or `Label` is able to see how much is owed to them,
and how the earned monies are to be allocated between the service operator, the payment
processor, and the `Artist` or `Label`. Furthermore, an `Artist` or `Label` is
able to see what the payout fee will be the next time a payout occurs. All fees
are articulated clearly and transparently.

In the event of transaction reversals (chargebacks and refunds), the fees
assessed are itemized and explained clearly.

#### Chargebacks

A "chargeback" occurs when a customer disputes a purchase and the customer's bank
reverses the transaction, thereby forcing the merchant to return the funds.

In consideration of this service, in particular, chargebacks are exceedingly
rare, for several reasons:

1. When dealing with intangible products, all the shipping-related reasons that
lead to chargebacks (e.g., lost and stolen packages) do not apply, and a customer
cannot reasonably claim, "I never received the item" unless there is a technical
problem with the file delivery/download mechanism.
2. Digital audio downloads are not "high-dollar" items, so the incentive for so-called
["Friendly Fraud"](https://chargebacks911.com/chargebacks/#cbFriendlyFraud) simply
does not exist, thereby reducing the likelihood of chargebacks even further.
3. The service operator is abundantly cautious with regard to fraud. Any time a
suspicious transaction occurs, it is reviewed carefully, and if deemed risky,
the transaction is refunded immediately to mitigate the risk of a chargeback.
Given that there is no "loss in inventory" when dealing with digital products,
erring on the side of caution (by issuing a refund) when a transaction smells
the least bit like fraud is the best approach.

Despite the extremely unlikely event of a chargeback, a written policy is required
for the sake of transparency.
 
The service operator incurs a $20.00 USD fee for a chargeback, which is passed
on to the `Artist`(s) in the form of a balance reduction. If the `Artist` is not
already owed a balance that is sufficient to cover the chargeback fee, a negative
balance results. While the service operator will not demand a monetary payment
from the `Artist` to reconcile the negative balance, any future sales will be
applied to the negative balance before the `Artist` is eligible to be paid out.

In the event that more than one `Artist's` `Songs` are included in an `Order`
that is subjected to a chargeback, the associated fee will be allocated in
proportion to each `Artist's` share of the `Order` total.

Given that a chargeback can be initiated for up to 90 days after the initial
purchase, and possibly longer, and the matter could remain unresolved for many
months after that, it is entirely possible that an `Artist` or `Label` has already
been paid out in relation to a given transaction before the chargeback fee is
assessed. For this reason, the service operator may, at any time, assess a
chargeback fee in relation to a previous transaction.

#### Refunds

In relation to this service, in particular, requests for a refund are nearly as
rare as chargebacks. In the vast majority of cases, refunds are issued because
a transaction appears to be fraudulent, i.e., made with a stolen payment card.

In the event that the service operator decides, at its sole discretion, to issue
a refund to a customer, the transaction will simply be reversed and the associated
monies debited from the balance owed to the `Artist` or `Label`.

The only nuance to refunds is that the payment processor does not refund the fees
associated with the original transaction. Accordingly, the service operator passes
these fees on to the `Artist` or `Label`. As with chargebacks, this can result in
a negative balance owed to the `Artist` or `Label` (see `Chargebacks` section
for additional details as to the implications of a negative balance).

In the event that more than one `Artist's` `Songs` are included in an `Order`
that is refunded to the customer, the fee associated with the original payment
will be allocated in proportion to each `Artist's` share of the `Order` total.

Refunds share another property of chargebacks, which is that they can be issued
well after the intial purchase was made, and can therefore be assessed after
and `Artist` or `Label` has already been paid out in relation to a given transaction.
For this reason, the service operator may, at any time, issue a refund to a customer
in relation to a previous transaction, and adjust the balance owed to an
`Artist` or `Label` accordingly.

### Record-Keeping Requirements

In terms of the required accounting, the `Order` metadata includes the following
metrics of relevance:

- The total purchase price.
- The price charged for each associated `OrderItem`.
- The payment processor fees charged (with detailed currency conversion information,
  if applicable).

The `Payout` metadata includes:

- The gross income associated with the inbound `Orders`.
- The sum and itemized details of all **inbound** payment processor fees associated therewith.
- The sum and itemized details of all service operator fees associated therewith.
- The sum and itemized details of all **outbound** payment processor fees associated therewith.
- The calculated net earnings (gross income, less each of the aforementioned fees).

For a comprehensive overview of the fees associated with each type of payment
processor transaction, see: [PayPal Transaction Fees](https://www.paypal.com/us/webapps/mpp/merchant-fees#paypal-payouts)

### Payments to Artists

Whenever a balance is owed to an `Artist`, it is eligible to be paid on the
last day of the month, provided the balance is above the minimum withdrawal
threshold that the service operator specifies. This threshold is necessary to ensure that
the monetary transfer does not incur fees in excess of the amount to be
transferred (in which case the service would lose money on the transaction).

The following rules apply:

1. `Artist` is paid monthly, on the last day of the month.
2. `Artist` must have at least $5.00 balance in order to be paid.
3. `Artist` may specify a withdrawal threshold in excess of $5.00, up to the
   maximum supported transaction amount ($20,000 USD, or equivalent in any
   other currency). See: [Payouts Fees](https://developer.paypal.com/docs/payouts/reference/fees/)

### Payments to Labels

Payments made to `Labels` differ from those made to `Artists` only in that they
are a "roll-up" of the money owed to all of the `Artists` that are associated
with the `Label`. In other words, a single payment is made to the `Label`, instead
of multiple, smaller payments being made to each `Artist` associated with the
`Label` individually.

_However_, `Labels` do have the option of specifying that each of their `Artists`
is paid individually and directly, rather than through the `Label`. The `Label`
may set this preference on a per-`Artist` basis. So, for example, if a `Label`
has 20 `Artists`, it could specify that five of them are paid directly, and the
`Label` receives a single payment for the other 15 `Artists` and then distributes
the money as appropriate.

The service provides this flexibility because some `Labels` take a fee of their
own before paying their `Artists`, while others do not, or may have a different
compensation arrangement altogether.

### How Artist & Label Payments Are Processed

On the last day of each month, a scheduled task is run, which computes the payouts
that are owed to each `Artist` and `Label`.

Crucially, this task is "idempotent"; that is, it can be run any number of times
without changing the result. Consequently, if the task is not run on-time, or fails
to any extent, it can be run again, or even repeatedly, until it completes. Upon
completion, the results for the period (month) in question are stored, and any
subsequent attempt to calculate the results for the same period will be ignored.

For such a task to function, it must receive the target (destination) period,
i.e., the month and year, as arguments. For example:

```
php artisan payout:calculate --month=1 --year=2021
```

The task runs within the context of a database transaction, so that if it fails
in any capacity, the computations are discarded completely. If run successfully,
the `Payout` and `PayoutDetail` results are stored with the following metadata:

`Payout` (one row for _all_ payout transactions for the period):

- Month
- Year
- Sum of all fields described in `Payments to Artists & Labels`

`PayoutDetail` (one row for _each_ `CatalogEntity` to be paid):

- All fields described in `Payments to Artists & Labels`
- `CatalogEntity` ID

Additionally, a service operator staff member must review and approve each
`PayoutDetail` record in order for the associated payment to be made. Upon approval,
the staff member's `user_id` is recorded, along with other relevant metadata.

## User Privacy

The service operator complies with GDPR and all applicable laws in the countries
in which it operates.

## Security

- All payment information is secured through the third-party payment processor's
  (PayPal's) network.
- Customer payment information is never stored; transactions are processed in
  real-time and only the results (pass/fail and amount) are stored.
- `User` passwords are stored securely, in hashed format, using the most current
  encryption standards.
