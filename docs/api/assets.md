Asset Exchange Operations
---------------------------

### Cancel Order

Cancel an existing asset order. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is either _cancelBidOrder_ or _cancelAskOrder_
*   _order_ is the order ID of the order being canceled

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Cancel Order](API_Examples.md#cancel-order "The Blue0x API Examples") example.

#### Cancel Ask Order

Refer to [Cancel Order](#cancel-order "The Blue0x API").

#### Cancel Bid Order

Refer to [Cancel Order](#cancel-order "The Blue0x API").

### Delete Asset Shares

Permanently deletes a specified quantity of owned asset shares.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _deleteAssetShares_
*   _asset_ is the asset ID
*   _quantityQNT_ is the quantity (in QNT) of the asset to be deleted

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Asset Shares](API_Examples.md#delete-asset-shares "The Blue0x API Examples") example.

### Dividend Payment

Pay dividend to all shareholders of an asset. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dividendPayment_
*   _asset_ is the asset ID
*   _height_ is the blockchain height at which asset holders shares will be counted (must be less than 1440 blocks in the past)
*   _amountNQTPerQNT_ is dividend amount (in NQT per QNT of the asset)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Dividend Payment](API_Examples.md#dividend-payment "The Blue0x API Examples") example.

### Get Account Asset Count

Get the number of assets owned by an account given the account ID.

**Request:**

*   _requestType_ is _getAccountAssetCount_
*   _account_ is the account ID
*   _height_ is the height of the blockchain to determine the asset count (optional, default is last block)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _numberOfAssets_ (N) is the number of assets owned by the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Asset Count](API_Examples.md#get-account-asset-count "The Blue0x API Examples") example.

### Get Account Assets

Get the assets owned by a given account in reverse quantity order.

**Request:**

*   _requestType_ is _getAccountAssets_
*   _account_ is the account ID
*   _asset_ is an asset ID filter (optional)
*   _height_ is the blockchain height at which to retrieve balances (optional, default is the last block in the blockchain)
*   _includeAssetInfo_ is _true_ if asset information is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _accountAssets_ (A) is an array of asset objects (unless the _asset_ parameter is specified) with the following fields for each asset:
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset
    *   _unconfirmedQuantityQNT_ (S) is the unconfirmed quantity (in QNT) of the asset
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _name_ (S) is the asset name (if _includeAssetInfo_ is _true_)
    *   _asset_ (S) is the asset ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Assets](API_Examples.md#get-account-assets "The Blue0x API Examples") example.

### Get Account Current Order Ids

Get current asset order IDs given an account ID in reverse block height order.

**Request:**

*   _requestType_ is either _getAccountCurrentBidOrderIds_ or _getAccountCurrentAskOrderIds_
*   _account_ is the account ID
*   _asset_ is an asset ID filter (optional)
*   _firstIndex_ is a zero-based index to the first order ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrderIds_ or _askOrderIds_ (A) is an array of order IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Current Order Ids](API_Examples.md#get-account-current-order-ids "The Blue0x API Examples") example.

#### Get Account Current Ask Order Ids

Refer to [Get Account Current Order Ids](#get-account-current-order-ids "The Blue0x API").

#### Get Account Current Bid Order Ids

Refer to [Get Account Current Order Ids](#get-account-current-order-ids "The Blue0x API").

### Get Account Current Orders

Get current asset orders given an account ID in reverse block height order.

**Request:**

*   _requestType_ is either _getAccountCurrentBidOrders_ or _getAccountCurrentAskOrders_
*   _account_ is the account ID
*   _asset_ is an asset ID filter (optional)
*   _firstIndex_ is a zero-based index to the first order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrders_ or _askOrders_ (A) is an array of order objects (refer to [Get Order](#get-order "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Current Orders](API_Examples.md#get-account-current-orders "The Blue0x API Examples") example.

#### Get Account Current Ask Orders

Refer to [Get Account Current Orders](#get-account-current-orders "The Blue0x API").

#### Get Account Current Bid Orders

Refer to [Get Account Current Orders](#get-account-current-orders "The Blue0x API").

### Get All Assets

Get all assets in the exchange in reverse block height of creation order.

**Request:**

*   _requestType_ is _getAllAssets_
*   _firstIndex_ is a zero-based index to the first asset to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Assets](API_Examples.md#get-all-assets "The Blue0x API Examples") example.

### Get All Open Orders

Get all open bid/ask orders in reverse block height order.

**Request:**

*   _requestType_ is either _getAllOpenBidOrders_ or _getAllOpenAskOrders_
*   _firstIndex_ is a zero-based index to the first order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _openOrders_ (A) is an array of order objects (refer to [Get Order](#get-order "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Open Orders](API_Examples.md#get-all-open-orders "The Blue0x API Examples") example.

#### Get All Open Ask Orders

Refer to [Get All Open Orders](#get-all-open-orders "The Blue0x API").

#### Get All Open Bid Orders

Refer to [Get All Open Orders](#get-all-open-orders "The Blue0x API").

### Get All Trades

Get all trades since a given timestamp in reverse block height order.

**Request:**

*   _requestType_ is _getAllTrades_
*   _timestamp_ is the timestamp (in seconds since the genesis block) to begin retrieving trades (optional, default 0)
*   _firstIndex_ is a zero-based index to the first trade to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last trade to retrieve (optional)
*   _includeAssetInfo_ is _true_ if asset information is to be included in the result (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If _timestamp_ is omitted or zero, and no index is given, all trades in the entire blockchain will be retrieved, which may timeout or crash your system.

**Response:**

*   _trades_ (A) is an array of trade objects (refer to [Get Trades](#get-trades "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Trades](API_Examples.md#get-all-trades "The Blue0x API Examples") example.

### Get Asset

Get asset information given an asset ID.

**Request:**

*   _requestType_ is _getAsset_
*   _asset_ is the asset ID
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _account_ (S) is the number of the account that issued the asset
*   _accountRS_ (S) is the Reed-Solomon address of the account that issued the asset
*   _name_ (S) is the asset name
*   _description_ (S) is the asset description
*   _quantityQNT_ (S) is the total asset quantity (in QNT) in existence
*   _asset_ (N) is the asset ID
*   _decimals_ (N) is the number of decimal places used by the asset
*   _numberOfAccounts_ (N) is the number of accounts that own the asset
*   _numberOfTrades_ (N) is the number of trades of this asset
*   _numberOfTransfers_ (N) is the number of transfers of this asset
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset](API_Examples.md#get-asset "The Blue0x API Examples") example.

### Get Asset Account Count

Get the number of accounts that own an asset given the asset ID.

**Request:**

*   _requestType_ is _getAssetAccountCount_
*   _asset_ is the asset ID
*   _height_ is the height of the blockchain to determine the account count (optional, default is last block)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _numberOfAccounts_ (N) is the number of accounts that own the asset
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Account Count](API_Examples.md#get-asset-account-count "The Blue0x API Examples") example.

### Get Asset Accounts

Get the accounts that own an asset given the asset ID in reverse quantity order.

**Request:**

*   _requestType_ is _getAssetAccounts_
*   _asset_ is the asset ID
*   _height_ is the height of the blockchain to determine the accounts (optional, default is last block)
*   _firstIndex_ is a zero-based index to the first account to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last account to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _accountAssets_ (A) is an array of asset objects with the following fields for each asset:
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset
    *   _accountRS_ (S) is the Reed-Solomon address of the account that owns the asset
    *   _unconfirmedQuantityQNT_ (S) is the unconfirmed quantity (in QNT) of the asset
    *   _asset_ (S) is the asset ID
    *   _account_ (S) is the number of the account that owns the asset
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Accounts](API_Examples.md#get-asset-accounts "The Blue0x API Examples") example.

### Get Asset Deletes

Get asset deletions for a specific asset or account.

**Request:**

*   _requestType_ is _getAssetDeletes_
*   _asset_ is the asset ID (optional if account is provided)
*   _account_ is the account ID (optional if asset is provided)
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _timestamp_ is the earliest deletion (in seconds since the genesis block) to retrieve (optional)
*   _includeAssetInfo_ is _true_ if asset information is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _deletes_ (A) is an array of asset delete objects with following properties:
    *   _quantityQNT_ (S) is the number of shares that was deleted
    *   _assetDelete_ (S) is the transaction ID
    *   _account_ (S) is the account ID
    *   _accountRS_ (S) is the account Reed Solomon address
    *   _asset_ (S) is the asset ID
    *   _height_ (N) is the block height of the delete
    *   _timestamp_ (N) is the block timestamp of the delete
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _name_ (S) is the asset name (if _includeAssetInfo_ is _true_)

**Example:** Refer to [Get Asset Deletes](API_Examples.md#get-asset-deletes "The Blue0x API Examples") example.

### Get Asset Dividends

Get the dividend payment history for a specific asset.

**Request:**

*   _requestType_ is _getAssetDividends_
*   _asset_ is the asset ID
*   _firstIndex_ is a zero-based index to the first dividend payment to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last dividend payment to retrieve (optional)
*   _timestamp_ is the earliest dividend payment (in seconds since the genesis block) to retrieve (optional)
*   _adminPassword_ is a string with the admin password (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _dividends_ (A) is an array of dividend transactions with the following properties:
    *   _assetDividend_ (S) is the dividend payment transaction ID
    *   _numberOfAccounts_ (N) is the number of accounts that received a dividend
    *   _amountNQTPerQNT_ (S) is the amount of BLX (in NQT) paid per quantity (in QNT) of the asset
    *   _totalDividend_ (S) is the total amount of BLX (in NQT) sent in the dividend payment
    *   _dividendHeight_ (N) is the block height of the dividend calculation
    *   _asset_ (S) is the asset ID
    *   _height_ (N) is the block height of the dividend payment
    *   _timestamp_ (N) is the block timestamp of the dividend payment

**Example:** Refer to [Get Asset Dividends](API_Examples.md#get-asset-dividends "The Blue0x API Examples") example.

### Get Asset Ids

Get the IDs of all assets in the exchange in reverse block height of creation order.

**Request:**

*   _requestType_ is _getAssetIds_
*   _firstIndex_ is a zero-based index to the first asset ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Ids](API_Examples.md#get-asset-ids "The Blue0x API Examples") example.

### Get Asset Transfers

Get transfers associated with a given asset and/or account in reverse block height order (or in the expected order of execution for expected transfers).

**Request:**

*   _requestType_ is either _getAssetTransfers_ or _getExpectedAssetTransfers_, where expected transfers are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _asset_ is the asset ID (optional)
*   _account_ is the account ID (optional if _asset_ provided)
*   _timestamp_ is the earliest transfer (in seconds since the genesis block) to retrieve (optional, does not apply to expected transfers)
*   _firstIndex_ is a zero-based index to the first transfer to retrieve (optional, does not apply to expected transfers)
*   _lastIndex_ is a zero-based index to the last transfer to retrieve (optional, does not apply to expected transfers)
*   _includeAssetInfo_ is _true_ if the _decimals_ and _name_ fields are to be included (optional, does not apply to expected transfers)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transfers_ (A) is an array of transfer objects with the following fields for each transfer:
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset traded
    *   _senderRS_ (S) is the Reed-Solomon address of the sender
    *   _assetTransfer_ (S) is the transaction ID of the asset transfer
    *   _sender_ (S) is the account number of the sender
    *   _recipientRS_ (S) is the Reed-Solomon address of the recipient
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _recipient_ (S) is the account number of the recipient
    *   _name_ (S) is the name of the asset (if _includeAssetInfo_ is _true_)
    *   _asset_ (S) is the asset ID
    *   _height_ (N) is the height of the transfer block
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the transfer block, does not apply to an expected transfer
    *   _phased_ (B) is _true_ if the transaction is phased, _false_ otherwise, applies only to an expected transfer
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Transfers](API_Examples.md#get-asset-transfers "The Blue0x API Examples") example.

#### Get Expected Asset Transfers

Refer to [Get Asset Transfers](#get-asset-transfers "The Blue0x API").

### Get Assets

Get asset information given multiple asset IDs

**Request:**

*   _requestType_ is _getAssets_
*   _assets_ is one the multiple asset IDs
*   _assets_ is one the multiple asset IDs
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Assets](API_Examples.md#get-assets "The Blue0x API Examples") example.

### Get Assets By Issuer

Get asset information given multiple creation account IDs in reverse block height of creation order.

**Request:**

*   _requestType_ is _getAssetsByIssuer_
*   _account_ is one of the multiple account IDs
*   _account_ is one of the multiple account IDs
*   _firstIndex_ is a zero-based index to the first asset to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Assets By Issuer](API_Examples.md#get-assets-by-issuer "The Blue0x API Examples") example.

### Get Expected Asset Deletes

Gets asset deletes which are expected to be executed in the next block.

**Request:**

*   _requestType_ is either _getExpectedAssetDeletes_
*   _asset_ is the asset ID (optional)
*   _account_ is the account ID (optional)
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _timestamp_ is the earliest deletion (in seconds since the genesis block) to retrieve (optional)
*   _includeAssetInfo_ is _true_ if asset information is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _deletes_ (A) is an array of expected asset delete objects with following properties:
    *   _assetDelete_ (S) is the transaction ID
    *   _asset_ (S) is the asset ID
    *   _account_ (S) is the account ID
    *   _accountRS_ (S) is the account Reed Solomon address
    *   _quantityQNT_ (S) is the number of shares that will be deleted
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _name_ (S) is the asset name (if _includeAssetInfo_ is _true_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Expected Asset Deletes](API_Examples.md#get-expected-asset-deletes "The Blue0x API Examples") example.

### Get Order

Get a bid/ask order given an order ID.

**Request:**

*   _requestType_ is either _getBidOrder_ or _getAskOrder_
*   _order_ is the Order ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _account_ (S) is the account number associated with the order
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _asset_ (S) is the ID of the asset being ordered
*   _quantityQNT_ (S) is the order quantity (in QNT)
*   _priceNQT_ (S) is the order price (in NQT)
*   _height_ (N) is the block height of the order transaction
*   _transactionHeight_ (N) is the transaction height
*   _transactionIndex_ (N) is a zero-based index giving the order of the transaction in its block
*   _order_ (S) is the ID of the order
*   _type_ (S) is the type of order (_bid_ or _ask_)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Order](API_Examples.md#get-order "The Blue0x API Examples") example.

#### Get Ask Order

Refer to [Get Order](#get-order "The Blue0x API").

#### Get Bid Order

Refer to [Get Order](#get-order "The Blue0x API").

### Get Order Ids

Get bid/ask order IDs given an asset ID, in order of decreasing bid price or increasing ask price.

**Request:**

*   _requestType_ is either _getBidOrderIds_ or _getAskOrderIds_
*   _asset_ is the asset ID
*   _firstIndex_ is a zero-based index to the first order ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrderIds_ or _askOrderIds_ (A) is an array of order IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Order Ids](API_Examples.md#get-order-ids "The Blue0x API Examples") example.

#### Get Ask Order Ids

Refer to [Get Order Ids](#get-order-ids "The Blue0x API").

#### Get Bid Order Ids

Refer to [Get Order Ids](#get-order-ids "The Blue0x API").

### Get Orders

Get bid/ask orders given an asset ID, in order of decreasing bid price or increasing ask price (if _sortByPrice_ is _true_ for expected orders, otherwise in the expected order of execution).

**Request:**

*   _requestType_ is one of _getBidOrders_, _getAskOrders_, _getExpectedBidOrders_ or _getExpectedAskOrders_, where expected orders are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _asset_ is the asset ID
*   _sortByPrice_ is _true_ to sort by price (optional, applies only to expected orders, which are returned in expected order of execution by default)
*   _showExpectedCancellations_ is _true_ to include orders that are expected to be cancelled in the next block, based on the content of the unconfirmed transactions pool and the phased transactions expected to finish in the next block (optional, does not apply to expected orders)
*   _firstIndex_ is a zero-based index to the first order to retrieve (optional, does not apply to expected orders)
*   _lastIndex_ is a zero-based index to the last order to retrieve (optional, does not apply to expected orders)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrders_ or _askOrders_ (A) is an array of order objects (refer to [Get Order](#get-order "The Blue0x API") for details) with the following additional field only for an expected order:
    *   _phased_ (B) is _true_ if the order is phased, _false_ otherwise
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Orders](API_Examples.md#get-orders "The Blue0x API Examples") example.

#### Get Ask Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

#### Get Bid Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

#### Get Expected Ask Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

#### Get Expected Bid Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

### Get Expected Order Cancellations

Get all expected order cancellations in the order in which they are expected to be executed.

**Request:**

*   _requestType_ is _getExpectedOrderCancellations_, where expected cancellations are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _orderCancellations_ (A) is an array of order cancellation objects with the following fields for each transfer:
    *   _account_ (S) is the cancelling account number
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _order_ (S) is the ID of the order to be cancelled
    *   _height_ (N) is the block height of the order cancellation transaction
    *   _phased_ (B) is _true_ if the order cancellation transaction is phased
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Expected Order Cancellations](API_Examples.md#get-expected-order-cancellations "The Blue0x API Examples") example.

### Get Last Trades

Get the last trade of each of multiple assets.

**Request:**

*   _requestType_ is _getLastTrades_
*   _assets_ is one of multiple asset IDs
*   _assets_ is one of multiple asset IDs
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _trades_ (A) is an array of trade objects (refer to [Get Trades](#get-trades "The Blue0x API") without _name_ and _decimals_ for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Last Trades](API_Examples.md#get_last-trades "The Blue0x API Examples") example.

### Get Order Trades

Get all trades that were executed as a result of a given _askOrder_ and/or _bidOrder_ in reverse block height order.

**Request:**

*   _requestType_ is _getOrderTrades_
*   _askOrder_ is an ask order ID (optional)
*   _bidOrder_ is a bid order ID (optional if _askOrder_ provided)
*   _firstIndex_ is a zero-based index to the first trade to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last trade to retrieve (optional)
*   _includeAssetInfo_ is _true_ if the _decimals_ and _name_ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Trades](#get-trades "The Blue0x API").

**Example:** Refer to [Get Order Trades](API_Examples.md#get-order-trades "The Blue0x API Examples") example.

### Get Trades

Get trades associated with a given asset and/or account in reverse block height order.

**Request:**

*   _requestType_ is _getTrades_
*   _asset_ is the asset ID (optional)
*   _account_ is the account ID (optional if _asset_ provided)
*   _firstIndex_ is a zero-based index to the first trade to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last trade to retrieve (optional)
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _includeAssetInfo_ is _true_ if the _decimals_ and _name_ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _trades_ (A) is an array of trade objects with the following fields for each trade:
    *   _seller_ (S) is the account number of the seller
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset traded
    *   _bidOrder_ (S) is the bid order ID
    *   _sellerRS_ (S) is the Reed-Solomon address of the seller
    *   _buyer_ (S) is the account number of the buyer
    *   _priceNQT_ (S) is the trade price (in NQT, the ask price for a buy or the bid price for a sell)
    *   _askOrder_ (S) is the ask order ID
    *   _buyerRS_ (S) is the Reed-Solomon address of the buyer
    *   _decimals_ (N) is the number of decimal places used by the asset
    *   _name_ (S) is the name of the asset (if _includeAssetInfo_ is _true_)
    *   _block_ (S) is the block ID of the trade (if _includeAssetInfo_ is _true_)
    *   _asset_ (S) is the asset ID
    *   _askOrderHeight_ (N) is the block height of the ask order
    *   _bidOrderHeight_ (N) is the block height of the bid order
    *   _tradeType_ (S) is the trade type (_sell_ or _buy_, where _buy_ implies that the bid occurred after the ask, or if in the same block, has a greater order ID)
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the trade block
    *   _height_ (N) is the height of the trade block
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Trades](API_Examples.md#get-trades "The Blue0x API Examples") example.

### Issue Asset

Create an asset on the exchange. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _issueAsset_
*   _name_ is the name of the asset
*   _description_ is a url-encoded description of the asset in UTF-8 with a maximum length of 1000 bytes (optional)
*   _quantityQNT_ is the total amount (in QNT) of the asset in existence
*   _decimals_ is the number of decimal places used by the asset (optional, zero default)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the asset ID.

**Example:** Refer to [Issue Asset](API_Examples.md#issue-asset "The Blue0x API Examples") example.

### Place Order

Place an asset order. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is either _placeBidOrder_ or _placeAskOrder_
*   _asset_ is the asset ID of the asset being ordered
*   _quantityQNT_ is the amount (in QNT) of the asset being ordered
*   _priceNQT_ is the bid/ask price (in NQT)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the order ID.

**Example:** Refer to [Place Order](API_Examples.md#place-order "The Blue0x API Examples") example.

#### Place Ask Order

Refer to [Place Order](#place-order "The Blue0x API").

#### Place Bid Order

Refer to [Place Order](#place-order "The Blue0x API").

### Search Assets

Get assets having a name or description that match a given query in reverse relevance order.

**Request:**

*   _requestType_ is _searchAssets_
*   _query_ is a full text query on the asset fields _name_ (S) and _description_ (S) in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _firstIndex_ is a zero-based index to the first asset to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Assets](API_Examples.md#search-assets "The Blue0x API Examples") example.

### Transfer Asset

Transfer a quantity of an asset from one account to another. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _transferAsset_
*   _recipient_ is the recipient account ID
*   _recipientPublicKey_ is the public key of the recipient account (optional, enhances security of a new account)
*   _asset_ is the ID of the asset being transferred
*   _quantityQNT_ is the amount (in QNT) of the asset being transferred

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the transfered asset ID.

**Example:** Refer to [Transfer Asset](API_Examples.md#transfer-asset "The Blue0x API Examples") example.