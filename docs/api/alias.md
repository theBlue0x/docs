Alias Operations
------------------

### Buy / Sell Alias

Buy or sell an alias. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is either _buyAlias_ or _sellAlias_
*   _alias_ is the ID of the alias (optional)
*   _aliasName_ is the alias name (optional if _alias_ provided)
*   _priceNQT_ is the asking price (in NQT) of the alias (_sellAlias_ only)
*   _amountNQT_ is the amount (in NQT) offered for an alias for sale (_buyAlias_ only)
*   _recipient_ is the account ID of the recipient (only required for _sellAlias_ and only if there is a designated recipient)
*   _recipientPublicKey_ is the public key of the recipient account (only applicable if _recipient_ provided; optional, enhances security of a new account)

**Note**: An alias can be transferred rather than sold by setting _priceNQT_ to zero. A pending sale can be canceled by selling again to self for a price of zero.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Buy / Sell Alias](API_Examples.md#buy-sell-alias "The Blue0x API Examples") example.

#### Buy Alias

Refer to [Buy / Sell Alias](#buy-sell-alias "The Blue0x API").

#### Sell Alias

Refer to [Buy / Sell Alias](#buy-sell-alias "The Blue0x API").

### Set Alias

Create and/or assign an alias. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setAlias_
*   _aliasName_ is the alias name
*   _aliasURI_ is the alias URI (e.g. [https://www.google.com/](https://www.google.com/))

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the alias ID.

**Example:** Refer to [Set Alias](API_Examples.md#set-alias "The Blue0x API Examples") example.

### Delete Alias

Delete an alias given an alias ID or name. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _deleteAlias_
*   _alias_ is the alias ID (optional)
*   _aliasName_ is the alias name to be deleted (optional if _alias_ provided)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Alias](API_Examples.md#delete-alias "The Blue0x API Examples") example.

### Get Alias

Get information about a given alias

**Request:**

*   _requestType_ is _getAlias_
*   _alias_ is the alias ID (optional)
*   _aliasName_ is the name of the alias (optional if _alias_ provided)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _timestamp_ (N) is the time (in seconds since the genesis block) when the alias was created or last transferred
*   _aliasName_ (S) is the name of the alias
*   _account_ (S) is the number of the account that owns the alias
*   _accountRS_ (S) is the Reed-Solomon address of the account that owns the alias
*   _aliasURI_ (S) is what the alias points to, in URI format
*   _alias_ (S) is the alias ID
*   _priceNQT_ (S) is the asking price (in NQT) of the alias if it is for sale
*   _buyer_ (S) is the account number of the buyer if the alias is for sale and a buyer is specified
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Alias](API_Examples.md#get-alias "The Blue0x API Examples") example.

### Get Alias Count

Get the number of aliases owned by an account given the account ID.

**Request:**

*   _requestType_ is _getAliasCount_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfAliases_ (N) is the number of aliases owned by the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Alias Count](API_Examples.md#get-alias-count "The Blue0x API Examples") example.

### Get Aliases

Get information on aliases owned by a given account in alias name order.

**Request:**

*   _requestType_ is _getAliases_
*   _account_ is the ID of the account that owns the aliases
*   _timestamp_ is the earliest creation time (in seconds since the genesis block) of the aliases (optional)
*   _firstIndex_ is a zero-based index to the first alias to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last alias to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _aliases_ (A) is an array of alias objects (refer to [Get Alias](#get-alias "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Aliases](API_Examples.md#get-aliases "The Blue0x API Examples") example.

### Get Aliases Like

Get all aliases starting with a given prefix in alias name order.

**Request:**

*   _requestType_ is _getAliasesLike_
*   _aliasPrefix_ is the prefix (at least 2 characters long) of the _aliasName_
*   _firstIndex_ is a zero-based index to the first alias to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last alias to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _aliases_ (A) is an array of alias objects (refer to [Get Alias](#get-alias "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Aliases Like](API_Examples.md#get-aliases-like "The Blue0x API Examples") example.