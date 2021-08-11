Monetary System Operations
-----------------------------

### Can Delete Currency

Determine if a currency can be deleted.

**Request:**

*   _requestType_ is _canDeleteCurrency_
*   _account_ is the account ID
*   _currency_ is the currency ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _canDelete_ (B) is _true_ if the currency can be deleted, _false_ otherwise
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** a currency can be deleted only when all units of the currency are held by _account_. A reservable currency that has not yet been issued can be deleted by the issuer. A mintable currency that has not completed minting cannot be deleted by a non-issuer.

**Example:** Refer to [Can Delete Currency](API_Examples.md#can-delete-currency "The Blue0x API Examples") example.

### Currency Buy / Sell

Make an exchange request to buy or sell an exchangeable currency. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyBuy_ or _currencySell_
*   _currency_ is the currency ID
*   _rateNQT_ is the exchange rate (in NQT per QNT)
*   _units_ is the amount of the currency to buy or sell (in QNT)

**Note:** An exchange request is immediately executed once accepted onto the blockchain based only on currently available offers (refer to [Publish Exchange Offer](#publish-exchange-offer "The Blue0x API")). The request then expires, regardless of the amount of currency exchanged; the request may be completely filled, partially filled, or expire without any exchange if no matching offers are found.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Buy / Sell](API_Examples.md#currency-buy-sell "The Blue0x API Examples") example.

#### Currency Buy

Refer to [Currency Buy / Sell](#currency-buy-sell "The Blue0x API").

#### Currency Sell

Refer to [Currency Buy / Sell](#currency-buy-sell "The Blue0x API").

### Currency Mint

Submit a valid computed nonce to the blockchain in return for newly minted currency. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyMint_
*   _currency_ is the mintable currency ID
*   _nonce_ is the computed nonce
*   _units_ is the amount (in QNT) of currency to mint
*   _counter_ (N) is the counter associated with the minting account

**Note:** The hash of _nonce_ must be less than _targetBytes_ provided by [Get Minting Target](#get-minting-target "The Blue0x API") for given _units_ and _counter_. _counter_ must be increased with each submission.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Mint](API_Examples.md#currency-mint "The Blue0x API Examples") example.

### Currency Reserve Claim

Claim currency reserve. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyReserveClaim_
*   _currency_ is the currency ID
*   _units_ is the amount (in QNT) of reserve currency to claim

**Note:** Holders of a claimable currency may claim the locked NQT backing their units, thus reducing the supply of the currency.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Reserve Claim](API_Examples.md#currency-reserve-claim "The Blue0x API Examples") example.

### Currency Reserve Increase

Increase the currency reserve of an unissued reservable currency. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyReserveIncrease_
*   _currency_ is the currency ID
*   _amountPerUnitNQT_ is the additional amount (in NQT per QNT of _reserveSupply_) to reserve (refer to [Issue Currency](#issue-currency "The Blue0x API"))

**Note:** An additional _amountPerUnitNQT_ \* _reserveSupply_ NQT (beyond what has previously been reserved) will be locked until the _issuanceHeight_ is reached. Upon issuance, if the currency is claimable the NQT will remain locked until claimed; otherwise the NQT will transfer to the issuing account. Also upon issuance, a portion of the _reserveSupply_ QNT will be transfered to each reserving account in proportion to the NQT that was contributed.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Reserve Increase](API_Examples.md#currency-reserve-increase "The Blue0x API Examples") example.

### Delete Currency

Delete a deletable currency (refer to [Can Delete Currency](#can-delete-currency "The Blue0x API")). POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _deleteCurrency_
*   _currency_ is the currency ID

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Currency](API_Examples.md#delete-currency "The Blue0x API Examples") example.

### Get Account Currencies

Get the currencies issued by a given account.

**Request:**

*   _requestType_ is _getAccountCurrencies_
*   _account_ is the account ID
*   _currency_ is a currency ID filter (optional)
*   _height_ is the blockchain height at which the response applies (optional, default is the current height)
*   _includeCurrencyInfo_ is _true_ if several currency information properties is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _accountCurrencies_ (A) is an array of currency objects with the following fields:
    *   _code_ (S) is the currency code
    *   _unconfirmedUnits_ (S) is the amount of unconfirmed currency units (in QNT)
    *   _decimals_ (N) is the number of currency decimal places
    *   _name_ (S) is the currency name
    *   _currency_ (S) is the currency ID
    *   _units_ (S) is the amount of currency (in QNT)
    *   _issuanceHeight_ (N) is the blockchain height of issuance for a reservable currency
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API"))
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Currencies](API_Examples.md#get-account-currencies "The Blue0x API Examples") example.

### Get Account Currency Count

Get the number of currencies issued by a given account.

**Request:**

*   _requestType_ is _getAccountCurrencyCount_
*   _account_ is the account ID
*   _height_ is the blockchain height at which the response applies (optional, default is the current height)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfCurrencies_ (N) is the number of currencies issued
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Currency Count](API_Examples.md#get-account-currency-count "The Blue0x API Examples") example.

### Get Account Exchange Requests

Get the exchange requests associated with a given account and/or currency in reverse chronological order (or in expected order of execution for expected requests).

**Request:**

*   _requestType_ is either _getAccountExchangeRequests_ or _getExpectedExchangeRequests_, where expected requests are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _account_ is the account ID (optional for expected requests)
*   _currency_ is the currency ID (optional for expected requests if _account_ provided)
*   _includeCurrencyInfo_ is _true_ to include the response fields _code_, _decimals_, _name_, _issuanceHeight_, _type_, _timestamp_, _issuerAccountRS_ and _issuerAccount_ (optional, applies only to expected requests)
*   _firstIndex_ is a zero-based index to the first request to retrieve (optional, does not apply to expected requests)
*   _lastIndex_ is a zero-based index to the last request to retrieve (optional, does not apply to expected requests)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchangeRequests_ (A) is an array of requests with the following fields for each request:
    *   _code_ (S) is a currency code
    *   _subtype_ (N) is _5_ for buy and _6_ for sell
    *   _decimals_ (N) is the number of decimal places
    *   _name_ (S) is the currency name
    *   _units_ (S) is the number of currency units to buy or sell (in QNT)
    *   _issuanceHeight_ (N) is the blockchain height of issuance for a reservable currency, zero otherwise
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API"))
    *   _transaction_ (S) is the transaction ID
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the block when the request was executed
    *   _rateNQT_ (S) is the exchange rate (in NQT per QNT)
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
    *   _phased_ (B) is true if the transaction is phased (applies only to expected requests)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Exchange Requests](API_Examples.md#get-account-exchange-requests "The Blue0x API Examples") example.

#### Get Expected Exchange Requests

Refer to [Get Account Exchange Requests](#get-account-exchange-requests "The Blue0x API").

### Get Funding Monitor

Get a funding monitor.

**Request:**

*   _requestType_ is _getFundingMonitor_
*   _secretPhrase_ is the secret phrase of the funding account, used to get a single monitor. (optional)
*   _adminPassword_ is the admin password, used to get a single monitor or all monitors (optional if _secretPhrase_ is provided)
*   _includeMonitoredAccounts_ is _true_ to include account info of the monitored accounts (optional)
*   _property_ is the name of the account property (optional)
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)
*   _account_ is the account ID (optional)

**Response:**

*   _monitors_ (A) is an array of monitor objects including the following fields:
    *   _holding_ (S) is the holding ID
    *   _amount_ (S) is the amount to fund the monitored accounts with (NQT or QNT)
    *   _monitoredAccounts_ (A) is an array of monitored account objects including the following fields (only if _includeMonitoredAccounts_ is _true_):
        *   _amount_ (S) is the amount to fund the monitored accounts with. Overrides amount in parent object.
        *   _account_ (S) is the monitored account ID
        *   _accountRS_ (S) is the monitored account Reed Solomon address
        *   _threshold_ (S) is the threshold. Overrides threshold in parent object.
        *   _interval_ (N) is the interval. Overrides interval in parent object.
    *   _holdingType_ (N) is the holding type
    *   _account_ (S) is the monitoring account ID
    *   _accountRS_ (S) is the Reed Solomon address of the monitoring account
    *   _property_ (S) is the account property
    *   _threshold_ (S) is the threshold
    *   _interval_ (N) is the interval
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Funding Monitor](API_Examples.md#get-funding-monitor "The Blue0x API Examples") example.

### Get All Currencies

Get all currencies in reverse creation order.

**Request:**

*   _requestType_ is _getAllCurrencies_
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _includeCounts_ is _true_ to include _numberOf..._ fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of currency objects (refer to [Get Currency](#get-currency "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Currencies](API_Examples.md#get-all-currencies "The Blue0x API Examples") example.

### Get All Exchanges

Get all currency exchanges in reverse chronological order.

**Request:**

*   _requestType_ is _getAllExchanges_
*   _timestamp_ is the earliest timestamp to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first exchange to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last exchange to retrieve (optional)
*   _includeCurrencyInfo_ is _true_ to include some currency details (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Exchanges](API_Examples.md#get-all-exchanges "The Blue0x API Examples") example.

### Get Available To Buy

Calculates the rate required in order to completely fill an exchange request.

**Request:**

*   _requestType_ is _getAvailableToBuy_ or _getAvailableToSell_
*   _currency_ is the currency ID
*   _units_ is the number of units to buy
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _amountNQT_ (S) is the total amount needed to fill the exchange request
*   _units_ (S) is the number of units
*   _rateNQT_ (S) is the rate for the currency units
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Available To Buy](API_Examples.md#get-available-to-buy "The Blue0x API Examples") example.

#### Get Available To Sell

Refer to [Get Available To Buy](#get-available-to-buy "The Blue0x API").

### Get Buy / Sell Offers

Get currency buy or sell offers given a currency ID and/or an account ID in order of rate (if _sortByRate_ is _true_ for expected offers, otherwise in the expected order of execution).

**Request:**

*   _requestType_ is one of _getBuyOffers_, _getSellOffers_, _getExpectedBuyOffers_ or _getExpectedSellOffers_, where expected offers are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _currency_ is the currency ID (optional)
*   _account_ is the account ID (optional if _currency_ provided)
*   _availableOnly_ is _true_ to include only offers with non-zero supply and limit, but is ignored when both _currency_ and _account_ are given (optional, does not apply to expected offers)
*   _sortByRate_ is _true_ to sort by rate (optional, applies only to expected offers, which are returned in expected order of execution by default)
*   _firstIndex_ is a zero-based index to the first offer to retrieve (optional, does not apply to expected offers)
*   _lastIndex_ is a zero-based index to the last offer to retrieve (optional, does not apply to expected offers)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _offers_ (A) is an array of buy or sell offer objects (refer to [Get Offer](#get-offer "The Blue0x API") for details) with the following additional field only for an expected offer:
    *   _phased_ (B) is _true_ if the offer is phased, _false_ otherwise
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Buy / Sell Offers](API_Examples.md#get-buy-sell-offers "The Blue0x API Examples") example.

#### Get Buy Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

#### Get Sell Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

#### Get Expected Buy Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

#### Get Expected Sell Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

### Get Currencies

Get currencies given multiple currency IDs.

**Request:**

*   _requestType_ is _getCurrencies_
*   _currencies_ is one of multiple currency IDs
*   _currencies_ is one of multiple currency IDs

⋮

*   _includeCounts_ is _true_ to include _numberOf..._ fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of currency objects (refer to [Get Currency](#get-currency "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currencies](API_Examples.md#get-currencies "The Blue0x API Examples") example.

### Get Currencies By Issuer

Get currencies issued by multiple accounts in reverse creation order.

**Request:**

*   _requestType_ is _getCurrenciesByIssuer_
*   _account_ is one of multiple account IDs
*   _account_ is one of multiple account IDs
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _includeCounts_ is _true_ if _numberOf..._ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of arrays of currency objects, where the outer array is indexed by the given account IDs (refer to [Get Currency](#get-currency "The Blue0x API") for details about the currency objects)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currencies By Issuer](API_Examples.md#get-currencies-by-issuer "The Blue0x API Examples") example.

### Get Currency

Get the details of a currency.

**Request:**

*   _requestType_ is _getCurrency_
*   _currency_ is the currency ID (optional)
*   _code_ is the currency code (optional if _currency_ provided)
*   _includeCounts_ is _true_ if _numberOf..._ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _initialSupply_ (S) is the initial currency supply (in QNT)
*   _currentReservePerUnitNQT_ (S) is the minimum currency reserve (in NQT per QNT)
*   _types_ (A) is an array of currency types, one or more of:
    *   _EXCHANGEABLE_
    *   _CONTROLLABLE_
    *   _RESERVABLE_
    *   _CLAIMABLE_
    *   _MINTABLE_
    *   _NON-SHUFFLEABLE_
*   _code_ (S) is the currency code
*   _creationHeight_ (N) is the blockchain height of the currency creation
*   _minDifficulty_ (N) is the minimum difficulty for a mintable currency
*   _numberOfTransfers_ (N) is the number of currency transfers
*   _description_ (S) is the currency description
*   _minReservePerUnitNQT_ (S) is the minimum currency reserve (in NQT per QNT) for a reservable currency
*   _currentSupply_ (S) is the current currency supply (in QNT)
*   _issuanceHeight_ (N) is the blockchain height of the currency issuance for a reservable currency
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _type_ (N) is the currency type bitmask, from least to most significant bit: exchangeable, controllable, reservable, claimable, mintable, non-shuffleable
*   _reserveSupply_ (S) is the reserve currency supply (in NQT) for a reservable currency
*   _maxDifficulty_ (N) is the maximum difficulty for a mintable currency
*   _accountRS_ (S) is the Reed-Solomon address of the issuing account
*   _decimals_ (N) is the number of decimal places used by the currency
*   _name_ (S) is the name of the currency
*   _numberOfExchanges_ (N) is the number of currency exchanges
*   _currency_ (S) is the currency ID
*   _maxSupply_ (S) is the maximum currency supply (in QNT)
*   _account_ (S) is the account ID of the currency issuer
*   _algorithm_ (N) is the algorithm number for a mintable currency: 2 for SHA256, 3 for SHA3, 5 for Scrypt, 25 for Keccak25
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)

**Example:** Refer to [Get Currency](API_Examples.md#get-currency "The Blue0x API Examples") example.

### Get Currency Account Count

Get the number of accounts that hold a given currency.

**Request:**

*   _requestType_ is _getCurrencyAccountCount_
*   _currency_ is the currency ID
*   _height_ is the blockchain height at which the response applies (optional, default is the current height)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfAccounts_ (N) is the number of accounts that hold the currency
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Account Count](API_Examples.md#get-currency-account-count "The Blue0x API Examples") example.

### Get Currency Accounts

Get the accounts that hold a given currency in reverse units order.

**Request:**

*   _requestType_ is _getCurrencyAccounts_
*   _currency_ is the currency ID
*   _height_ is the blockchain height at which the response applies (optional, default is current height)
*   _firstIndex_ is a zero-based index to the first account to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last account to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _accountCurrencies_ (A) is an array of account objects with the following fields:
    *   _unconfirmedUnits_ (S) is the amount of unconfirmed currency units (in QNT)
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _currency_ (S) is the currency ID
    *   _units_ (S) is the amount of currency (in QNT)
    *   _account_ (S) is the account number
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Accounts](API_Examples.md#get-currency-accounts "The Blue0x API Examples") example.

### Get Currency Founders

Get a reservable currency's founders.

**Request:**

*   _requestType_ is _getCurrencyFounders_
*   _currency_ is the currency ID
*   _account_ is an account ID (optional)
*   _firstIndex_ is a zero-based index to the first founder to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last founder to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _founders_ (A) is an array of founder objects each of which has the following fields:
    *   _accountRS_ (S) is the Reed-Solomon address of the founding account
    *   _amountPerUnitNQT_ (S) is the amount (in NQT per QNT of _reserveSupply_) reserved by the founder
    *   _currency_ (S) is the currency ID
    *   _account_ (S) is the founding account number
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Founders](API_Examples.md#get-currency-founders "The Blue0x API Examples") example.

### Get Currency Ids

Get all currency IDs in reverse chronological creation order.

**Request:**

*   _requestType_ is _getCurrencyIds_
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencyIds_(A) is an array of currency IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Ids](API_Examples.md#get-currency-ids "The Blue0x API Examples") example.

### Get Currency Transfers

Get currency transfers given a currency ID and/or an account ID in reverse block height order (or in expected order of execution for expected transfers).

**Request:**

*   _requestType_ is either _getCurrencyTransfers_ or _getExpectedCurrencyTransfers_, where expected transfers are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _currency_ is the currency ID (optional)
*   _account_ is the account ID (optional if _currency_ provided)
*   _timestamp_ is the earliest transfer (in seconds since the genesis block) to retrieve (optional, does not apply to expected transfers)
*   _firstIndex_ is a zero-based index to the first transfer to retrieve (optional, does not apply to expected transfers)
*   _lastIndex_ is a zero-based index to the last transfer to retrieve (optional, does not apply to expected transfers)
*   _includeCurrencyInfo_ is _true_ to include some currency fields (optional, does not apply to expected transfers)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transfers_ (A) is an array of transfer objects with the following fields for each transfer:
    *   _code_ (S) is the currency code
    *   _units_ (S) is the amount (in QNT) of the transfer
    *   _issuanceHeight_ (N) is the blockchain height of the currency issuance for a reservable currency
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API") for details)
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _transfer_ (S) is the transfer ID
    *   _senderRS_ (S) is the Reed-Solomon address of the sender account
    *   _sender_ (S) is the account number of the sender account
    *   _recipientRS_ (S) is the Reed-Solomon address of the recipient account
    *   _decimals_ (N) is the number of decimal places used by the currency
    *   _recipient_ (S) is the account number of the recipient account
    *   _name_ (S) is the currency name
    *   _currency_ (S) is the currency ID
    *   _issuerAccount_ (S) is the issuer account ID
    *   _height_ (N) is the blockchain height of the transfer
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the transfer block, does not apply to an expected transfer
    *   _phased_ (B) is _true_ if the transaction is phased, _false_ otherwise, applies only to an expected transfer
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Transfers](API_Examples.md#get-currency-transfers "The Blue0x API Examples") example.

#### Get Expected Currency Transfers

Refer to [Get Currency Transfers](#get-currency-transfers "The Blue0x API").

### Get Exchanges

Get currency exchanges given a currency and/or an account in reverse chronological order.

**Request:**

*   _requestType_ is _getExchanges_
*   _currency_ is a currency ID (optional)
*   _account_ is an account ID (optional if _currency_ provided)
*   _firstIndex_ is a zero-based index to the first currency exchange to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency exchange to retrieve (optional)
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _includeCurrencyInfo_ is _true_ to include several currency-related fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects with the following fields for each exchange:
    *   _seller_ (S) is the seller account number
    *   _code_ (S) is the currency code
    *   _sellerRS_ (S) is the Reed-Solomon address of the seller account
    *   _units_ (S) is the amount of currency exchanged (in QNT)
    *   _issuanceHeight_ (N) is the blockchain height of currency issuance for a reservable currency
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API") for details)
    *   _rateNQT_ (S) is the currency exchange rate (in NQT per QNT)
    *   _buyer_ (S) is the account number of the buyer
    *   _offer_ (S) is the offer ID
    *   _buyerRS_ (S) is the Reed-Solomon address of the buyer account
    *   _decimals_ (N) is the number of decimal places used by the currency
    *   _name_ (S) is the currency name
    *   _currency_ (S) is the currency ID
    *   _block_ (S) is the block ID of the block containing the exchange transaction
    *   _transaction_ (S) is the transaction ID of the exchange
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the exchange
    *   _height_ is the blockchain height of the block containing the exchange transaction
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Exchanges](API_Examples.md#get-exchanges "The Blue0x API Examples") example.

### Get Exchanges By Exchange Request

Get currency exchanges given an exchange request transaction ID in reverse chronological order.

**Request:**

*   _requestType_ is _getExchangesByExchangeRequest_
*   _transaction_ is the transaction ID of the exchange request
*   _includeCurrencyInfo_ is _true_ to include several currency-related fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Exchanges By Exchange Request](API_Examples.md#get-exchanges-by-exchange-request "The Blue0x API Examples") example.

### Get Exchanges By Offer

Get currency exchanges given a currency offer ID in reverse chronological order.

**Request:**

*   _requestType_ is _getExchangesByOffer_
*   _offer_ (S) is a currency offer ID
*   _includeCurrencyInfo_ is _true_ to include several currency-related fields (optional)
*   _firstIndex_ is a zero-based index to the first currency exchange to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency exchange to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Exchanges By Offer](API_Examples.md#get-exchanges-by-offer "The Blue0x API Examples") example.

### Get Last Exchanges

Get the last exchange of each of multiple currencies.

**Request:**

*   _requestType_ is _getLastExchanges_
*   _currencies_ is one of multiple currency IDs
*   _currencies_ is one of multiple currency IDs

⋮

*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") without _name_, _decimals_, _code_, _issuanceHeight_, _type_, _issuerAccountRS_ and _issuerAccount_ for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Last Exchanges](API_Examples.md#get-last-exchanges "The Blue0x API Examples") example.

### Get Minting Target

Get the current minting target of a mintable currency.

**Request:**

*   _requestType_ is _getMintingTarget_
*   _currency_ is the mintable currency ID
*   _account_ is the minting account ID
*   _units_ is the amount (in QNT) of currency to mint
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** _units_ cannot be greater than 1/10000 of the _maxSupply_ (refer to [Issue Currency](#issue-currency "The Blue0x API")). Increasing _units_ decreases _targetBytes_.

**Response:**

*   _difficulty_ (S) is the current difficulty, an estimate of the number of hashes needed to meet the target
*   _targetBytes_ (S) is the 32-byte target (little endian), which must equal or exceed the computed hash of the _nonce_
*   _currency_ (S) is the currency ID
*   _counter_ (N) is the counter associated with the minting account, the value previously submitted to [Currency Mint](#currency-mint "The Blue0x API")
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** If a _nonce_ is found such that its hash is less than the target, it can be submitted to the blockchain along with _counter_ + 1 using [Currency Mint](#currency-mint "The Blue0x API"), which results in _units_ NQT being credited to the minting account. _difficulty_ is inversely related to the target, and so increases exponentially as _maxSupply_ is approached because the target is defined as (2exp\-1)/_units_, where exp decreases linearly from 256-_minDifficulty_ to 256-_maxDifficulty_. (Refer to [Issue Currency](#issue-currency "The Blue0x API") for _maxSupply_, _minDifficulty_ and _maxDifficulty_.)

**Example:** Refer to [Get Minting Target](API_Examples.md#get-minting-target "The Blue0x API Examples") example.

### Get Offer

Get offer details given an offer ID.

**Request:**

*   _requestType_ is _getOffer_
*   _offer_ is the offer ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _sellOffer_ and _buyOffer_ (O) are objects with the following fields:
    *   _offer_ (S) is the offer ID
    *   _expirationHeight_ (N) is the blockchain height of offer expiration
    *   _accountRS_ (S) is the Reed-Solomon address of the offering account
    *   -limit_ (S) is the cumulative limit of currency buys or sells
    *   _currency_ (S) is the currency ID
    *   _supply_ (S) is the current currency supply
    *   _account_ (S) is the offering account number
    *   _height_ (N) is the blockchain height of offer creation
    *   _rateNQT_ (S) is the currency exchange rate (in NQT per QNT)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Offer](API_Examples.md#get-offer "The Blue0x API Examples") example.

### Issue Currency

Issue a new currency or re-issue an existing currency with different properties. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _issueCurrency_
*   _name_ is the currency name, 3 to 10 characters and longer than the currency code
*   _code_ is the uppercase currency code with the following fee structure: three letters 25000 BLX, four letters 1000 BLX, five letters 40 BLX, re-issue 40 BLX
*   _description_ is the currency description
*   _type_ is the currency type bitmask, from least to most significant bit: exchangeable, controllable, reservable, claimable, mintable, non-shuffleable
*   _initialSupply_ is the initial currency supply (in QNT) (must match _maxSupply_ unless mintable or claimable, must be zero for claimable)
*   _reserveSupply_ is the reserve currency supply (in QNT) (must match _maxSupply_)
*   _maxSupply_ is the maximum currency supply (in QNT)
*   _issuanceHeight_ is the blockchain height at which a reservable currency is issued if the reserve minimum is met
*   _minReservePerUnitNQT_ is the minimum currency reserve (in NQT per QNT of _reserveSupply_) for issuance of a reservable currency
*   _minDifficulty_ is the minimum difficulty (minimum _1_) for a mintable currency
*   _maxDifficulty_ is the maximum difficulty (maximum _255_ and greater than _minDifficulty_) for a mintable currency
*   _ruleset_ is for future use, always set to zero
*   _algorithm_ is an algorithm code for a mintable currency: _2_ for SHA256, _3_ for SHA3, _5_ for Scrypt, _25_ for Keccak25
*   _decimals_ is the number of decimal places used by the currency (optional, default zero)

**Notes:** Reservable requires exchangeable and/or claimable, as does controllable; but mintable requires exchangeable. Claimable requires reservable, non-mintable and zero _initialSupply_.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API"). The transaction ID is also the currency ID.

**Example:** Refer to [Issue Currency](API_Examples.md#issue-currency "The Blue0x API Examples") example.

### Publish Exchange Offer

Publish an exchange offer for an exchangeable currency. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _publishExchangeOffer_
*   _currency_ is the currency ID
*   _buyRateNQT_ is the offered buy rate (in NQT per QNT)
*   _sellRateNQT_ is the offered sell rate (in NQT per QNT)
*   _totalBuyLimit_ is the cumulative limit (in QNT) of currency buys
*   _totalSellLimit_ is the cumulative limit (in QNT) of currency sells
*   _initialBuySupply_ is the initial amount (in QNT) of currency offered to buy, cannot exceed _totalBuyLimit_
*   _initialSellSupply_ is the initial amount (in QNT) of currency offered to sell, cannot exceed _totalSellLimit_
*   _expirationHeight_ is the blockchain height for expiration of the offer

**Notes:** Each time currency is bought in response to an exchange request to sell currency (refer to [Currency Sell](#currency-buy-sell "The Blue0x API")), _totalBuyLimit_ is reduced and the supply of currency offered to sell increases by the amount bought. When _totalBuyLimit_ becomes zero, the buy offer is withdrawn. These same notes apply if _buy_ and _sell_ are interchanged. Only the most recent offer associated with an account is valid, even if an earlier offer by that account has not yet expired or reached its limits.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API"). The transaction ID is also the offer ID.

**Example:** Refer to [Publish Exchange Offer](API_Examples.md#publish-exchange-offer "The Blue0x API Examples") example.

### Search Currencies

Get currencies having a code that matches a given query in reverse relevance order.

**Request:**

*   _requestType_ is _searchCurrencies_
*   _query_ is a full text query on the currency field _code_ in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of currency objects (refer to [Get Currency](#get-currency "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Currencies](API_Examples.md#search-currencies "The Blue0x API Examples") example.

### Transfer Currency

Transfer currency to a given recipient. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _transferCurrency_
*   _recipient_ is the account ID of the transfer recipient
*   _currency_ is the currency ID
*   _units_ is the amount (in QNT) of the transfer

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Transfer Currency](API_Examples.md#transfer-currency "The Blue0x API Examples") example.