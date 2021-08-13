Digital Goods Store Operations
---------------------------------

In the BLX client interface, the Digital Goods Store (DGS) is referred to as the 'Marketplace'.

### DGS Delisting

Delist a listed product. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsDelisting_
*   _goods_ is the goods ID

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Delisting](API_Examples.md#dgs-delisting "The Blue0x API Examples") example.

### DGS Delivery

Deliver a product. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsDelivery_
*   _purchase_ is the purchase order ID
*   _discountNQT_ is a discount (in NQT) off the selling price (optional, default is zero)
*   _goodsToEncrypt_ is the product, a text or a hex string to be encrypted (optional if _goodsData_ provided)
*   _goodsIsText_ is _false_ if _goodsToEncrypt_ is a hex string (optional)
*   _goodsData_ is AES-encrypted (using [Encrypt To](messaging.md#encrypt-to "The Blue0x API")) _goodsToEncrypt_, up to 1000 bytes long (required only if _secretPhrase_ is omitted)
*   _goodsNonce_ is the unique nonce associated with the encrypted data (required only if _secretPhrase_ is omitted)

**Note:** If the encrypted goods data is longer than 1000 bytes, use a prunable encrypted message to deliver the goods.

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Delivery](API_Examples.md#dgs-delivery "The Blue0x API Examples") example.

### DGS Feedback

Give feedback about a purchased product after delivery. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsFeedback_
*   _purchase_ is the purchase order ID
*   _message_ is unencrypted (public) feedback text up to 1000 bytes

**Note**: The unencrypted _message_ parameter is used for public feedback.

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Feedback](API_Examples.md#dgs-feedback "The Blue0x API Examples") example.

### DGS Listing

List a product in the DGS by creating a listing transaction. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsListing_
*   _name_ is the name of the product up to 100 characters in length
*   _description_ is a description of the product up to 1000 characters in length
*   _tags_ are up to three comma separated keywords describing the product up to 100 characters in length (optional)
*   _quantity_ is the quantity of the product for sale
*   _priceNQT_ is the price (in NQT) of the product

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API"). The transaction ID is also the goods ID.

**Example:** Refer to [DGS Listing](API_Examples.md#dgs-listing "The Blue0x API Examples") example.

### DGS Price Change

Change the price of a listed product. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsPriceChange_
*   _goods_ is the goods ID of the product
*   _priceNQT_ is the new price of the product

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Price Change](API_Examples.md#dgs-price-change "The Blue0x API Examples") example.

### DGS Purchase

Purchase a product for sale. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsPurchase_
*   _goods_ is the goods ID of the product
*   _priceNQT_ is the price of the product
*   _quantity_ is the quantity to be purchased
*   _deliveryDeadlineTimestamp_ is the timestamp (in seconds since the genesis block) by which delivery of the product must occur

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API"). The transaction ID is also the purchase order ID.

**Example:** Refer to [DGS Purchase](API_Examples.md#dgs-purchase "The Blue0x API Examples") example.

### DGS Quantity Change

Change the quantity of a listed product. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsQuantityChange_
*   _goods_ is the goods ID of the product
*   _deltaQuantity_ is the change in the quantity of the product for sale (use negative numbers for a decrease in quantity)

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Quantity Change](API_Examples.md#dgs-quantity-change "The Blue0x API Examples") example.

### DGS Refund

Refund a purchase. POST only.

**Request:** Refer to [Create Transaction Request](create_transaction.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsRefund_
*   _purchase_ is the purchase order ID
*   _refundNQT_ is the amount (in NQT) of the refund

**Response:** Refer to [Create Transaction Response](create_transaction.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Refund](API_Examples.md#dgs-refund "The Blue0x API Examples") example.

### Get DGS Expired Purchases

Get purchase orders which have expired without being delivered, given a seller ID, in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSExpiredPurchases_
*   _seller_ is the account ID of the product seller
*   _firstIndex_ is a zero-based index to the first purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last purchase order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Expired Purchases](API_Examples.md#get-dgs-expired-purchases "The Blue0x API Examples") example.

### Get DGS Good

Get a DGS product given a goods ID.

**Request:**

*   _requestType_ is _getDGSGood_
*   _goods_ is the goods ID of the product
*   _includeCounts_ is _true_ if the fields beginning with numberOf... are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _seller_ (S) is the seller's account ID
*   _quantity_ (N) is the quantity of the product remaining for sale
*   _goods_ (S) is the ID of the product
*   _description_ (S) is the description of the product
*   _sellerRS_ (S) is the Reed-Solomon address of the seller's account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _delisted_ (B) is _true_ if the product has been delisted, _false_ otherwise
*   _parsedTags_ (A) is an array of up to three tag strings, parsed from the _tags_ field
*   _tags_ (S) is the comma separated list of tags provided by the seller when the listing was created
*   _priceNQT_ (S) is the current price of the product
*   _numberOfPublicFeedbacks_ (N) is the number of public feedbacks given for the product
*   _name_ (S) is the name of the product
*   _numberOfPurchases_ (N) is the number of purchases of the product
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the creation of the product listing
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)

**Example:** Refer to [Get DGS Good](API_Examples.md#get-dgs-good "The Blue0x API Examples") example.

### Get DGS Goods

Get DGS products for sale in reverse chronological listing creation order unless a seller is given, then in product name order.

**Request:**

*   _requestType_ is _getDGSGoods_
*   _seller_ is the account ID of the product seller (optional)
*   _firstIndex_ is a zero-based index to the first product to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last product to retrieve (optional)
*   _inStockOnly_ is _false_ if out-of-stock products (zero quantity) are to be retrieved (optional)
*   _hideDelisted_ is _true_ if delisted products are to be omitted (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If none of the optional parameters are specified, all in-stock products in the blockchain are retrieved at once, which may take a long time.

**Response:**

*   _goods_ (A) is an array of goods (refer to [Get DGS Good](#get-dgs-good "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Goods](API_Examples.md#get-dgs-goods "The Blue0x API Examples") example.

### Get DGS Goods Count

Get the number of products for sale by a given seller or all sellers.

**Request:**

*   _requestType_ is _getDGSGoodsCount_
*   _seller_ is the account ID of the seller (optional, default is all sellers combined)
*   _inStockOnly_ is _false_ if out-of-stock (zero quantity) products are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfGoods_ (N) is the number of goods for sale by the _seller_
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _numberOfGoods_ field refers to the number of distinct products for sale, regardless of the quantity of each.

**Example:** Refer to [Get DGS Goods Count](API_Examples.md#get-dgs-goods-count "The Blue0x API Examples") example.

### Get DGS Goods Purchase Count

Get the number of completed purchase orders given a goods ID.

**Request:**

*   _requestType_ is _getDGSGoodsPurchaseCount_
*   _goods_ is the goods ID
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfPurchases_ (N) is the number of completed purchase orders
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Goods Purchase Count](API_Examples.md#get-dgs-goods-purchase-count "The Blue0x API Examples") example.

### Get DGS Goods Purchases

Get completed purchase orders given a goods ID and optionally a buyer ID in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSGoodsPurchases_
*   _goods_ is the goods ID
*   _buyer_ is a buyer ID (optional)
*   _firstIndex_ is a zero-based index to the first purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last purchase order to retrieve (optional)
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Goods Purchases](API_Examples.md#get-dgs-goods-purchases "The Blue0x API Examples") example.

### Get DGS Pending Purchases

Get pending purchase orders given a seller ID in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSPendingPurchases_
*   _seller_ is the account ID of the seller
*   _firstIndex_ is a zero-based index to the first purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last purchase order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of pending purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Pending Purchases](API_Examples.md#get-dgs-pending-purchases "The Blue0x API Examples") example.

### Get DGS Purchase

Get a purchase order given a purchase order ID.

**Request:**

*   _requestType_ is _getDGSPurchase_
*   _purchase_ is the purchase order ID
*   _sharedKey_ is the shared key used to decrypt the message (optional) (see [Get Shared Key](messaging.md#get-shared-key "The Blue0x API"))
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _seller_ (S) is the account number of the seller
*   _quantity_ (N) is the quantity of the product to be purchased
*   _feedbackNotes_ (A) is an array of AES-encrypted objects, each with _data_ (S) and _nonce_ (S) fields, in reverse chronological order, if applicable
*   _publicFeedbacks_ (A) is an array of feedback strings in reverse chronological order if applicable
*   _pending_ (B) is _true_ if the _deliveryDeadline_ has not passed, _false_ otherwise
*   _purchase_ (S) is the purchase order ID
*   _goods_ (S) is the ID of the product
*   _sellerRS_ (S) is the Reed-Solomon address of the seller
*   _buyer_ (S) is the account number of the buyer
*   _priceNQT_ (S) is the price (in NQT) of the product
*   _deliveryDeadlineTimestamp_ (N) is the timestamp (in seconds since the genesis block) by which the product must be delivered
*   _goodsIsText_ (B) is _false_ if the message is a hex string, otherwise the message is text (optional)
*   _buyerRS_ (S) is the Reed-Solomon address of the buyer
*   _refundNQT_ (S) is the amount (in NQT) refunded, if applicable
*   _name_ (S) is the name of the product
*   _goodsData_ (O) is an object with the two fields _data_ (S) (the encrypted product hex string) and _nonce_ (S), if the product has been delivered
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the purchase order
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Purchase](API_Examples.md#get-dgs-purchase "The Blue0x API Examples") example.

### Get DGS Purchase Count

Get the number of purchase orders given a seller and/or buyer ID, or all orders combined.

**Request:**

*   _requestType_ is _getDGSPurchaseCount_
*   _seller_ is the account ID of the seller (optional, default is all sellers)
*   _buyer_ is the account ID of the buyer (optional, default is all buyers)
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfPurchases_ (N) is the number of purchase orders associated with the seller and/or buyer
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Purchase Count](API_Examples.md#get-dgs-purchase-count "The Blue0x API Examples") example.

### Get DGS Purchases

Get purchase orders given a seller and/or buyer ID in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSPurchases_
*   _seller_ is the account ID of the seller (optional)
*   _buyer_ is the account ID of the buyer (optional if _seller_ provided)
*   _firstIndex_ is a zero-based index to the purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the purchase order to retrieve (optional)
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Purchases](API_Examples.md#get-dgs-purchases "The Blue0x API Examples") example.

### Get DGS Tag Count

Get the number of tags used by all sellers.

**Request:**

*   _requestType_ is _getDGSTagCount_
*   _inStockOnly_ is _false_ if tags with no associated in-stock products are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfTags_ (N) is the number of tags
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Tag Count](API_Examples.md#get-dgs-tag-count "The Blue0x API Examples") example.

### Get DGS Tags

Get tags used by all sellers in reverse _inStockCount_, reverse _totalCount_, _tag_ order.

**Request:**

*   _requestType_ is _getDGSTags_
*   _inStockOnly_ is _false_ if out-of-stock tags are to be retrieved (optional)
*   _firstIndex_ is a zero-based index to the first tag to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tag to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _tags_ (A) is an array of tag objects with the following fields for each tag:
    *   _inStockCount_ (N) is the number of products available for sale as tagged
    *   _tag_ (S) is the tag word
    *   _totalCount_ (N) is the total number of products as tagged
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _...Count_ fields refer to the number of distinct products tagged, regardless of the quantity of each.

**Example:** Refer to [Get DGS Tags](API_Examples.md#get-dgs-tags "The Blue0x API Examples") example.

### Get DGS Tags Like

Get all tags starting with a given prefix (at least 2 characters long) in reverse _inStockCount_, reverse _totalCount_, _tag_ order.

**Request:**

*   _requestType_ is _getDGSTagsLike_
*   _tagPrefix_ is the prefix (at least 2 characters long) of the _tag_
*   _inStockOnly_ is _false_ if out-of-stock tags are to be retrieved (optional)
*   _firstIndex_ is a zero-based index to the first tag to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tag to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _tags_ (A) is an array of tag objects with the following fields for each tag:
    *   _inStockCount_ (N) is the number of products available for sale as tagged
    *   _tag_ (S) is the tag word
    *   _totalCount_ (N) is the total number of products as tagged
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _...Count_ fields refer to the number of distinct products tagged, regardless of the quantity of each.

**Example:** Refer to [Get DGS Tags Like](API_Examples.md#get-dgs-tags-like "The Blue0x API Examples") example.

### Search DGS Goods

Get product listings that have a name or description that match a given query in reverse relevance order, then name order (given a seller), then reverse chronological order.

**Request:**

*   _requestType_ is _searchDGSGoods_
*   _query_ is a full text query on the goods fields _name_ and _description_ in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview) (optional)
*   _tag_ is a query on the good field _tags_ in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview) (optional)
*   _seller_ is the account ID of the product seller (optional)
*   _firstIndex_ is a zero-based index to the first product to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last product to retrieve (optional)
*   _inStockOnly_ is _false_ if out-of-stock products (zero quantity) are to be retrieved (optional)
*   _hideDelisted_ is _true_ if delisted products are to be omitted (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _goods_ (A) is an array of goods objects (refer to [Get DGS Good](#get-dgs-good "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search DGS Goods](API_Examples.md#search-dgs-goods "The Blue0x API Examples") example.