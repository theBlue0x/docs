Account Operations
--------------------

### Delete Account Property

Deletes an account property. POST only.

**Request:**

*   _requestType_ is _deleteAccountProperty_
*   _property_ is the name of the property
*   _recipient_ is the account where a property should be removed (optional)
*   _setter_ is the account who did set the property (optional)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Account Property](API_Examples.md#delete-account-property "The Blue0x API Examples") example.

### Get Account

Get account information given an account ID.

**Request:**

*   _requestType_ is _getAccount_
*   _account_ is the account ID
*   _includeLessors_ is _true_ to include _lessors_, _lessorsRS_ and _lessorsInfo_ (optional)
*   _includeAssets_ is _true_ to include _assetBalances_ and _unconfirmedAssetBalances_ (optional)
*   _includeCurrencies_ is _true_ to include _accountCurrencies_ (optional)
*   _includeEffectiveBalance_ is _true_ to include _effectiveBalanceNXT_ and _guaranteedBalanceNQT_ (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _unconfirmedBalanceNQT_ (S) is _balanceNQT_ less unconfirmed outgoing transactions, the balance displayed in the client
*   _effectiveBalanceNXT_ (N) is the balance (in BLX) of the account available for forging: the unleased guaranteedBalance of this account plus the leased guaranteedBalance of all lessors to this account
*   _lessorsInfo_ (A) is an array of lessor objects including the fields:
    *   _currentHeightTo_ (S)
    *   _nextHeightFrom_ (S)
    *   _effectiveBalanceNXT_ (S)
    *   _nextLesseeRS_ (S)
    *   _currentLesseeRS_ (S)
    *   _currentHeightFrom_ (S)
    *   _nextHeightTo_ (S)
*   _lessors_ (A) is an array of lessor account IDs
*   _currentLessee_ (S) is the account number of the lessee, if applicable
*   _currentLeasingHeightTo_ (N) is the block height when the lease completes, if applicable
*   _forgedBalanceNQT_ (S) is the balance (in NQT) that the account has forged
*   _balanceNQT_ (S) is the minimally confirmed basic balance (in NQT) of the account
*   _publicKey_ (S) is the public key of the account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _assetBalances_ (A) is an array of asset objects including the fields _balanceQNT_ (S) and _asset_ (S) ID
*   _guaranteedBalanceNQT_ (S) is the balance (in NQT) of the account with at least 1440 confirmations
*   _unconfirmedAssetBalances_ (A) is an array of asset objects including the fields _unconfirmedBalanceQNT_ (S) and _asset_ (S) ID
*   _currentLesseeRS_ (S) is the Reed-Solomon address of the lessee account
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _lessorsRS_ (A) is an array of Reed-Solomon lessor account addresses
*   _accountCurrencies_ (A) is an array of currency objects (refer to [Get Account Currencies](monetary_system.md#get-account-currencies "The Blue0x API") for details)
*   _name_ (S) is the name associated with the account, if applicable
*   _description_ (S) is the description of the account, if applicable
*   _account_ (S) is the account number
*   _currentLeasingHeightFrom_ (N) is the block height when the lease starts, if applicable
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)

**Example:** Refer to [Get Account](API_Examples.md#get-account "The Blue0x API Examples") example.

### Get Account Block Count

Get the number of blocks forged by an account.

**Request:**

*   _requestType_ is _getAccountBlockCount_
*   _account_ is an account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfBlocks_ (N) is the number of blocks forged by the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Block Count](API_Examples.md#get-account-block-count "The Blue0x API Examples") example.

### Get Account Block Ids

Get the block IDs of all blocks forged (generated) by an account in reverse block height order.

**Request:**

*   _requestType_ is _getAccountBlockIds_
*   _account_ is the account ID
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first block ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blockIds_ (A) is an array of block IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millsec)

**Example:** Refer to [Get Account Block Ids](API_Examples.md#get-account-block-ids "The Blue0x API Examples") example.

### Get Account Blocks

Get all blocks forged (generated) by an account in reverse block height order.

**Request:**

*   _requestType_ is _getAccountBlocks_
*   _account_ is the account ID
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first block to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block to retrieve (optional)
*   _includeTransactions_ is _true_ to retrieve transaction details, otherwise only transaction IDs are retrieved (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blocks_ (A) is an array of blocks (refer to [Get Block](blocks.md#get-block "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Blocks](API_Examples.md#get-account-blocks "The Blue0x API Examples") example.

### Get Account Id

Get an account ID given a secret passphrase or public key. POST only.

**Request:**

*   _requestType_ is _getAccountId_
*   _secretPhrase_ is the secret passphrase of the account (optional)
*   _publicKey_ is the public key of the account (optional if _secretPhrase_ provided)

**Response:**

*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _publicKey_ (S) is the public key of the account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _account_ (S) is the account number

**Example:** Refer to [Get Account Id](API_Examples.md#get-account-id "The Blue0x API Examples") example.

### Get Account Ledger

Get multiple account ledger entries.

**Request:**

*   _requestType_ is _getAccountLedger_
*   _account_ is the account ID (optional)
*   _firstIndex_ is a zero-based index to the first block to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block to retrieve (optional)
*   _event_ is the event ID (optional)
*   _eventType_ is a string representing the event type (optional)
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)
*   _includeTransactions_ is true to retrieve transaction details, otherwise only transaction IDs are retrieved (optional)
*   _includeHoldingInfo_ is true to retrieve asset or currency info (optional) with each ledger entry. The default is false.
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _entries_ (A) is an array of ledger objects including the fields:
    *   _change_ (S) is the change in the balance for the holding identified by 'holdingType'
    *   _eventType_ (S) is the event type causing the account change
    *   _ledgerId_ (S) is the ledger entry ID
    *   _holding_ (S) is the item identifier for an asset or currency balance
    *   _isTransactionEvent_ (B) is true if the event is associated with a transaction and false if it is associated with a block.
    *   _balance_ (S) is the balance for the holding identified by 'holdingType'
    *   _holdingType_ (S) is the item being changed (account balance, asset balance or currency balance)
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _block_ (S) is the block ID that created the ledger entry
    *   _event_ (S) is the block or transaction associated with the event
    *   _account_ (S) is the account number
    *   _height_ (N) is the the block height associated with the event
    *   _timestamp_ (N) is the the block timestamp associated with the event
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Ledger](API_Examples.md#get-account-ledger "The Blue0x API Examples") example.

### Get Account Ledger Entry

Get a specific account ledger entry.

**Request:**

*   _requestType_ is _getAccountLedgerEntry_
*   _ledgerId_ is the ledger ID
*   _includeTransactions_ is true to retrieve transaction details, otherwise only transaction IDs are retrieved (optional)
*   _includeHoldingInfo_ is _true_ to retrieve asset or currency info (optional) with the ledger entry. The default is false.

**Response:**

*   _change_ (S) is the change in the balance for the holding identified by 'holdingType'
*   _eventType_ (S) is the event type causing the account change
*   _ledgerId_ (S) is the ledger entry ID
*   _holding_ (S) is the item identifier for an asset or currency balance
*   _isTransactionEvent_ (B) is true if the event is associated with a transaction and false if it is associated with a block.
*   _balance_ (S) is the balance for the holding identified by 'holdingType'
*   _holdingType_ (S) is the item being changed (account balance, asset balance or currency balance)
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _block_ (S) is the block ID that created the ledger entry
*   _event_ (S) is the block or transaction associated with the event
*   _account_ (S) is the account number
*   _height_ (N) is the the block height associated with the event
*   _timestamp_ (N) is the the block timestamp associated with the event
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Ledger Entry](API_Examples.md#get-account-ledger-entry "The Blue0x API Examples") example.

### Get Account Lessors

Get the lessors to an account.

**Request:**

*   _requestType_ is _getAccountLessors_
*   _account_ is the account ID
*   _height_ is the height of the blockchain to determine the lessors (optional, default is last block)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the _height_ must be within 1440 blocks of the last block.

**Response:**

*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _account_ (S) is the account number
*   _height_ (N) is the height of the blockchain
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _lessors_ (A) is an array of lessor objects including the fields:
    *   _lessorRS_ (S)
    *   _lessor_ (S)
    *   _guaranteedBalanceNQT_ (S)

**Example:** Refer to [Get Account Lessors](API_Examples.md#get-account_lessors "The Blue0x API Examples") example.

### Get Account Properties

Get the Account properties for a specific account or setter.

**Request:**

*   _requestType_ is _getAccountProperties_
*   _recipient_ is the account ID to which the property is attached to
*   _setter_ is the account ID who set the property (optional if _recipient_ provided)
*   _property_ is the property key (optional)
*   _firstIndex_ is a zero-based index to the first block to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _setterRS_: (S) is the Reed-Solomon address of the setter account (only if setter is defined in the request)
*   _recipientRS_: (S) is the Reed-Solomon address of the recipient account (only if recipient is defined in the request)
*   _recipient_: (S) is the account number of the recipient account (only if recipient is defined in the request)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _setter_: (S) is the account number of the setter account (only if setter is defined in the request)
*   _properties_: (A) is an array of properties including the fields:
    *   _setterRS_: (S) is the Reed-Solomon address of the setter account (only if setter is omitted in the request)
    *   _recipientRS_: (S) is the Reed-Solomon address of the recipient account (only if recipient is omitted in the request)
    *   _recipient_: (S) is the account number of the recipient account (only if recipient is omitted in the request)
    *   _property_: (S) property name
    *   _setter_: (S) is the account number of the setter account (only if setter is omitted in the request)
    *   _value_: (S) property value

**Example:** Refer to [Get Account Properties](API_Examples.md#get-account-properties "The Blue0x API Examples") example.

### Get Account Public Key

Get the public key associated with an account ID.

**Request:**

*   _requestType_ is _getAccountPublicKey_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _publicKey_ (S) is the 32-byte public key associated with the account, returned as a hex string
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Public Key](API_Examples.md#get-account-public-key "The Blue0x API Examples") example.

### Get Balance

Get the balance of an account.

**Request:**

*   _requestType_ is _getBalance_
*   _account_ is an account ID
*   _includeEffectiveBalance_ is _true_ to include _effectiveBalanceNXT_ and _guaranteedBalanceNQT_ (optional)
*   _height_ is the height to retrieve account balance for, if still available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _unconfirmedBalanceNQT_ (S) is _balanceNQT_ less unconfirmed outgoing transactions, the balance displayed in the client
*   _guaranteedBalanceNQT_ (S) is the balance (in NQT) of the account with at least 1440 confirmations
*   _effectiveBalanceNXT_ (N) is the balance (in Blue0x) of the account available for forging: the unleased guaranteedBalance of this account plus the leased guaranteedBalance of all lessors to this account
*   _forgedBalanceNQT_ (S) is the balance (in NQT) that the account has forged
*   _balanceNQT_ (S) is the minimally confirmed basic balance (in NQT) of the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Balance](API_Examples.md#get-balance "The Blue0x API Examples") example.

### Get Blockchain Transactions

Get the transactions associated with an account in reverse block timestamp order.

  
**Request:**

*   _requestType_ is _getBlockchainTransactions_
*   _account_ is the account ID
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _type_ is the type of transactions to retrieve (optional)
*   _subtype_ is the subtype of transactions to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last transaction to retrieve (optional)
*   _numberOfConfirmations_ is the required number of confirmations per transaction (optional)
*   _withMessage_ is _true_ to retrieve only only transactions having a message attachment, either non-encrypted or decryptable by the account (optional)
*   _phasedOnly_ is _true_ to retrieve only phased transactions (optional unless _nonPhasedOnly_ provided)
*   _nonPhasedOnly_ is _true_ to retrieve only nonphased transactions (optional unless _phasedOnly_ provided)
*   _includeExpiredPrunable_ is _true' to retrieve pruned data if available (optional)_
*   _includePhasingResult_ is _true_ to retrieve execution status of each phased transaction (optional)
*   _executedOnly_ is _true_ to exclude phased transactions that are not yet executed (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of transactions (refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Blockchain Transactions](API_Examples.md#get-blockchain-transactions "The Blue0x API Examples") example.

### Get Guaranteed Balance

Get the balance of an account that is confirmed at least a specified number of times.

**Request:**

*   _requestType_ is _getGuaranteedBalance_
*   _account_ is an account ID
*   _numberOfConfirmations_ is the minimum number of confirmations for a transaction to be included in the guaranteed balance (optional, if omitted or zero then minimally confirmed transactions are included)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _guaranteedBalanceNQT_ (S) is the balance (in NQT) of the account with at least _numberOfConfirmations_ confirmations
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Guaranteed Balance](API_Examples.md#get-guaranteed-balance "The Blue0x API Examples") example.

  

### Get Unconfirmed Transaction Ids

Get a list of unconfirmed transaction IDs associated with an account.

**Request:**

*   _requestType_ is _getUnconfirmedTransactionIds_
*   _account_ is one account ID (optional)
*   _account_ is one account ID (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)
*   _firstIndex_ is a zero-based index to the first transaction ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last transaction ID to retrieve (optional)

**Response:**

*   _unconfirmedTransactionIds_ (A) is an array of unconfirmed transaction IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Unconfirmed Transaction Ids](API_Examples.md#get-unconfirmed-transaction-ids "The Blue0x API Examples") example.

### Get Unconfirmed Transactions

Get a list of unconfirmed transactions associated with an account.

**Request:**

*   _requestType_ is _getUnconfirmedTransactions_
*   _account_ is one account ID (optional)
*   _account_ is one account ID (optional)

⋮

*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)
*   _firstIndex_ is a zero-based index to the first unconfirmed transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last unconfirmed transaction to retrieve (optional)

**Response:**

*   _unconfirmedTransactions_ (A) is an array of unconfirmed transactions (refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Unconfirmed Transactions](API_Examples.md#get-unconfirmed-transactions "The Blue0x API Examples") example.

### Search Accounts

Get accounts having a name or description that match a given query in reverse relevance order.

**Request:**

*   _requestType_ is _searchAccounts_
*   _query_ is a full text query on the account fields _name_ (S) and _description_ (S) in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _firstIndex_ is a zero-based index to the first account to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last account to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _accounts_ (A) is an array of account objects with the following fields:
    *   _account_ (S) is the account number
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _name_ (S) is the name of the account
    *   _description_ (S) is the description of the account (if applicable)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Accounts](API_Examples.md#search-accounts "The Blue0x API Examples") example.

### Send Money

Send Blue0x to another account. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _sendMoney_
*   _amountNQT_ is the amount (in NQT) in the transaction
*   _recipient_ is the account ID of the recipient
*   _recipientPublicKey_ is the public key of the receiving account (optional, enhances security of a new account)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Send Money](API_Examples.md#send-money "The Blue0x API Examples") example.

#### Send BLX

Refer to [Send Money](account.md#send-money "The Blue0x API").

### Set Account Info

Set account information. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setAccountInfo_
*   _name_ is a name to associate with the account (optional)
*   _description_ is a description to associate with the account (optional)
*   _messagePatternRegex_ is a regular expression pattern following the java.util.regex.Pattern specification; incoming transactions are only accepted if they contain a plain text message which matches this pattern (disabled indefinitely due to a security issue)
*   _messagePatternFlags_ is a bitmask of java.util.regex.Pattern flags, such as 2 (Pattern.CASE\-iNSENSITIVE)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Set Account Info](API_Examples.md#set-account-info "The Blue0x API Examples") example.

### Set Account Property

Set account property. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setAccountProperty_
*   _recipient_ is the account ID of the recipient.
*   _property_ is the property name.
*   _value_ is the property value.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Set Account Property](API_Examples.md#set-account-property "The Blue0x API Examples") example.

### Start Funding Monitor

Starts a funding monitor that will transfer BLX, an ASSET or a CURRENCY from the funding account to a recipient account when the amount held by the recipient account drops below the threshold. The transfer will not be done until the current block height is greater than equal to the block height of the last transfer plus the interval. The funding account is identified by the secret phrase.

The recipient accounts are identified by the specified account property (see [Set Account Property](account.md#set-account-property "The Blue0x API")). Each account that has this property set by the funding account will be monitored for changes. The property value can be omitted or it can consist of a JSON string containing one or more values in the format: {"amount":long,"threshold":long,"interval":integer} . POST only.

**Request:**

*   _requestType_ is _startFundingMonitor_
*   _property_ is the name of the account property
*   _amount_ is the amount to fund the recipient account with (in NQT or QNT)
*   _threshold_ is the threshold
*   _interval_ is the the number of blocks to wait after before funding the recipient
*   _secretPhrase_ is the secret phrase of the funding account
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)

**Response:**

*   _started_ (B) is _true_ if the monitor has been started
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Start Funding Monitor](API_Examples.md#start-funding-monitor "The Blue0x API Examples") example.

### Stop Funding Monitor

Stop a funding monitor. When the secret phrase is specified, a single monitor will be stopped. The monitor is identified by the secret phrase, holding and account property. The administrator password is not required and will be ignored.

When the administrator password is specified, a single monitor can be stopped by specifying the funding account, holding and account property. If no account is specified, all monitors will be stopped.

The holding type and account property name must be specified when the secret phrase or account is specified. Holding type codes are listed in getConstants. In addition, the holding identifier must be specified when the holding type is ASSET or CURRENCY. POST only.

**Request:**

*   _requestType_ is _stopFundingMonitor_
*   _secretPhrase_ is the secret phrase of the funding account, used to stop a single monitor. (optional)
*   _adminPassword_ is the admin password, used to stop a single monitor or all monitors (optional if _secretPhrase_ is provided)
*   _property_ is the name of the account property (optional)
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)
*   _account_ is the account ID (optional)

**Response:**

*   _stopped_ (N) is the number of the monitors that have been stopped
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Stop Funding Monitor](API_Examples.md#stop-funding-monitor "The Blue0x API Examples") example.

Account Control Operations
----------------------------

### Get All Phasing Only Controls

Retrieve all accounts subject to phasing control with their respective restrictions.

**Request:**

*   _requestType_ is _getAllPhasingOnlyControls_
*   _firstIndex_ is a zero-based index to the first block ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _phasingOnlyControls_ (A) is an array with phasing only controls objects (Refer to [Get Phasing Only Control](#get-phasing-only-control "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Phasing Only Controls](API_Examples.md#get-all-phasing-only-controls "The Blue0x API Examples") example.

### Get Phasing Only Control

Retrieve phasing control with their respective restrictions for a specific account.

**Request:**

*   _requestType_ is _getPhasingOnlyControl_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _account_ (S) is the account number
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _quorum_ (S) is the minimum number of votes needed to approve the transaction
*   _whitelist_ (A) is an array with the whitelisted accounts including the fields:
    *   _whitelisted_ (S) is the account number
    *   _whitelistedRS_ (S) is the Reed-Solomon address of the account
*   _maxFees_ (S) is the maximum fees the account can spend per block
*   _minDuration_ (N) is the minimum duration of the phasing period
*   _maxDuration_ (N) is the maximum duration of the phasing period
*   _votingModel_ (N) is an integer code for the method of approval
*   _minBalance_ (S) is the minimum balance (in NQT or QNT) required for voting
*   _minBalanceModel_ (N) is the minimum balance model
*   _holding_ (S) is the asset or currency ID (only included if holding != 0)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Phasing Only Control](API_Examples.md#get-phasing-only-control "The Blue0x API Examples") example.

### Set Phasing Only Control

Sets (or removes) phasing control for a specific account. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setPhasingOnlyControl_
*   _controlVotingModel_ is the voting model or -1 to remove phasing control
*   _controlQuorum_ is the expected quorum (optional)
*   _controlMinBalance_ is the expected minimum balance (optional)
*   _controlMinBalanceModel_ is the expected minimum balance model (optional)
*   _controlHolding_ is the holding ID (optional)
*   _controlWhitelisted_ is the whitelisted accounts (optional, multiple values)
*   _controlWhitelisted_ is the whitelisted accounts (optional, multiple values)
*   _controlMaxFees_ is the maximum allowed accumulated total fees for not yet finished phased transactions (optional)
*   _controlMinDuration_ is the minimum duration in block height (optional)
*   _controlMaxDuration_ is the maximum phasing duration in block height (optional)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Set Phasing Only Control](API_Examples.md#set-phasing-only-control "The Blue0x API Examples") example.