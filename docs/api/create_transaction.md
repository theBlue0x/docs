<h2><span class="mw-headline" id="Create_Transaction">Create Transaction</span></h2>
<p>The following API calls create BLX transactions using HTTP POST requests. Follow the links for examples and HTTP POST request parameters specific to each call. Calls in <i>italics</i> are phasing-safe (refer to <a href="/api/api/phasing/#create-phasing-poll">Create Phasing Poll</a>)
</p>
<ul><li><i><a href="/api/account#send-money">Send Money</a></i></li>
<li><i><a href="/api/account#set-account-info" title="Accounts">Set Account Information</a></i></li>
<li><i><a href="/api/account#set-account-property" title="Accounts">Set Account Property</a></i></li>
<li><a href="/api/alias#buy-sell-alias" title="Aliases">Buy / Sell Alias</a></li>
<li><a href="/api/alias#delete-alias" title="Aliases">Delete Alias</a></li>
<li><a href="/api/alias#set-alias" title="Aliases">Set Alias</a></li>
<li><a href="/api/messaging#send-message" title="Messages">Send Message</a></li>
<li><i><a href="/api/assets#cancel-order" title="Asset Exchange">Cancel Order</a></i></li>
<li><i><a href="/api/assets#dividend-payment" title="Asset Exchange">Dividend Payment</a></i></li>
<li><i><a href="/api/assets#issue-asset" title="Asset Exchange">Issue Asset</a></i></li>
<li><i><a href="/api/assets#place-order" title="Asset Exchange">Place Order</a></i></li>
<li><i><a href="/api/assets#transfer-asset" title="Asset Exchange">Transfer Asset</a></i></li>
<li><i><a href="/api/marketplace#dgs-delisting" title="Digital Goods Store">DGS Delisting</a></i></li>
<li><a href="/api/marketplace#dgs-delivery" title="Digital Goods Store">DGS Delivery</a></li>
<li><a href="/api/marketplace#dgs-feedback" title="Digital Goods Store">DGS Feedback</a></li>
<li><i><a href="/api/marketplace#dgs-listing" title="Digital Goods Store">DGS Listing</a></i></li>
<li><a href="/api/marketplace#dgs-price-change" title="Digital Goods Store">DGS PriceChange</a></li>
<li><a href="/api/marketplace#dgs-purchase" title="Digital Goods Store">DGS Purchase</a></li>
<li><a href="/api/marketplace#dgs-quantity-change" title="Digital Goods Store">DGS Quantity Change</a></li>
<li><a href="/api/marketplace#dgs-refund" title="Digital Goods Store">DGS Refund</a></li>
<li><i><a href="/api/forging#lease-balance" title="Forging">Lease Balance</a></i></li>
<li><a href="/api/monetary_system#currency-buy-sell" title="Monetary System">Currency Buy / Sell</a></li>
<li><a href="/api/monetary_system#currency-mint" title="Monetary System">Currency Mint</a></li>
<li><a href="/api/monetary_system#currency-reserve-claim" title="Monetary System">Currency Reserve Claim</a></li>
<li><a href="/api/monetary_system#currency-reserve-increase" title="Monetary System">Currency Reserve Increase</a></li>
<li><a href="/api/monetary_system#delete-currency" title="Monetary System">Delete Currency</a></li>
<li><a href="/api/monetary_system#issue-currency" title="Monetary System">Issue Currency</a></li>
<li><a href="/api/monetary_system#publish-exchange-offer" title="Monetary System">Publish Exchange Offer</a></li>
<li><a href="/api/monetary_system#transfer-currency" title="Monetary System">Transfer Currency</a></li>
<li><a href="/api/voting#create-poll" title="Voting System">Create Poll</a></li>
<li><a href="/api/voting#cast-vote" title="Voting System">Cast Vote</a></li>
<li><i><a href="/api/phasing#approve-transaction" title="Phasing">Approve Transaction</a></i></li>
<li><a href="/api/tagged_data#extend-tagged-data" title="Tagged Data">Extend Tagged Data</a></li>
<li><a href="/api/tagged_data#upload-tagged-data" title="Tagged Data">Upload Tagged Data</a></li></ul>


### Create Transaction Request
The following HTTP POST parameters are common to all API calls that create transactions:

*   _secretPhrase_ is the secret passphrase of the account (optional, but transaction neither signed nor broadcast if omitted)
*   _publicKey_ is the public key of the account (optional if _secretPhrase_ provided)
*   _feeNQT_ is the fee (in NQT) for the transaction:
    *   minimum 1000 BLX for [Issue Asset](assets.md#issue-asset "The Blue0x API"), unless singleton asset is issued, for which the fee is 1 BLX
    *   2 BLX in base fee for [Set Alias](alias.md#set-alias "The Blue0x API"), with 2 BLX additional fee for each 32 chars of name plus URI total length, after the first 32 chars
    *   \[25000, 1000, 40\] BLX for \[3-letter, 4-letter, 5-letter\] [Issue Currency](monetary_system.md#issue-currency "The Blue0x API")
    *   40 BLX for re-issue of any currency
    *   10 BLX for a [Create Poll](voting.md#create-poll "The Blue0x API"), including 20 options and total size of poll name plus poll description plus total option length not exceeding 320 chars. For each option above 20, an additional fee of 1 BLX, and for each 32 chars after 320, an additional fee of 2 BLX.
    *   \[2, 21\] BLX for a \[basic, required-minimum-balance\] [Create Phasing Poll](phasing.md#create-phasing-poll "The Blue0x API"). 1 BLX will be added for each option (answer) beyond 20, and 1 BLX for each 32 bytes of hashedSecret or linkedFullHash fields.
    *   1 BLX for the first 32 bytes of a unencrypted non-prunable [message](messaging.md#send-message "The Blue0x API"), 1 BLX for each additional 32 bytes
    *   2 BLX for the first 32 bytes of an encrypted non-prunable [message](messaging.md#send-message "The Blue0x API"), 1 BLX for each additional 32 bytes. The length is measured excluding the nonce and the 16 byte AES initialization vector.
    *   1 BLX for the first 1024 bytes of a prunable [message](messaging.md#send-message "The Blue0x API"), 0.1 BLX for each additional 1024 prunable bytes
    *   1 BLX for [Set Account Info](account.md#set-account-info "The Blue0x API"), including 32 chars, with 2 BLX additional fee for each 32 chars
    *   2 BLX for [Marketplace Listing](marketplace.md#dgs-listing "The Blue0x API"), including 32 chars of name plus description. With 2 BLX additional fee for each 32 chars.
    *   1 BLX for [Marketplace Delivery](marketplace.md#dgs-delivery "The Blue0x API"), including 32 bytes of encrypted goods data (AES initialization bytes and nonce excluded). With 2 BLX additional fee for each 32 bytes.
    *   2 BLX for transactions that make use of _referencedTransactionFullHash_ property when creating a new transaction.
    *   1 BLX otherwise, where 1 BLX = 100000000 NQT
*   _deadline_ is the deadline (in minutes) for the transaction to be confirmed, 32767 minutes maximum
*   _referencedTransactionFullHash_ creates a chained transaction, meaning that the current transaction cannot be confirmed unless the referenced transaction is also confirmed (optional)
*   _broadcast_ is set to _false_ to prevent broadcasting the transaction to the network (optional)

**Note:** An optional arbitrary message can be appended to any transaction by adding message-related parameters as in [Send Message](messaging.md#send-message "The Blue0x API").

**Note:** Any phasing-safe transaction (refer to [Create Transaction](create_transaction.md#create-transaction "The Blue0x API")) can have its execution deferred either conditionally or unconditionally (refer to [Create Phasing Poll](phasing.md#create-phasing-poll "The Blue0x API")).

### Create Transaction Response

The following JSON response fields are common to all API calls that create transactions:

*   _signatureHash_ (S) is a SHA-256 hash of the transaction signature
*   _unsignedTransactionBytes_ (S) are the unsigned transaction bytes
*   _transactionJSON_ (O) is a transaction object (refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details)
*   _broadcasted_ (B) is _true_ if the transaction was broadcast, _false_ otherwise
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _transactionBytes_ (S) are the signed transaction bytes
*   _fullHash_ (S) is the full hash of the signed transaction
*   _transaction_ (S) is the ID of the newly created transaction
