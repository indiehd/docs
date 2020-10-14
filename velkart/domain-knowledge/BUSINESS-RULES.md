## Adding Items/Products to the Cart

Velkart leverages the `bumbummen99/shoppingcart` package to provide its basic
shopping cart functionality. Velkart provides a lightweight wrapper to abstract
the third-party cart package's functionality, and also provides
integration tests to ensure interoperability.

When a customer adds an item to their `Cart`, a request is made to the
appropriate Velkart API endpoint, which delegates the requested operation to the
underlying repository.

## Placing an Order ("Checking Out")

When a customer proceeds to checkout, all of the `Cart` item details are
compared to their corresponding `Product` details -- `price` in particular --
and the customer is notified if the value has changed since the item was added
to the `Cart`.

This comparison is necessary in order to avoid querying the `Product` record for
every item in the cart every time the `Cart` is retrieved. Avoiding these
queries improves performance (because the `Cart` can then be cached for read
operations), but it also prevents the price of an item in the `Cart` from
quietly changing over time without the customer's awareness.

Once the customer has verified the `Cart` contents and price total, the customer
provides payment information, which is transmitted securely to a third-party
processing API. The API request specifies an immediate capture against the
customer's method of payment (rather than merely placing a hold to be collected
later). In most jurisdictions, immediate captures of this nature are lawful only
when the purchased item(s) does not need to be shipped. Given that we're dealing
only with digital products to which the customer gains access immediately upon
paying, an immediate capture is not only acceptable, but ideal.

If the third-party processor rejects the payment, the reason is logged and the
customer is informed of the relevant details. For security and fraud prevention
reasons, failure feedback is phrased in a manner that makes it as useful as
possible to a legitimate customer who may simply have made a typo in their
payment information, but minimally useful to an individual attempting to commit
fraud.

If the third-party processor accepts the payment, an `Order` is created, the
purchased `Products` and `Payment` are associated with it, and the customer is
able to access any `DigitalAssets` associated with the `Products` in the
`Order`.