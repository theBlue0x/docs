Transaction Operations
-------------------------

### Broadcast Transaction

Broadcast a transaction to the network. POST only.

**Request:**

*   _requestType_ is _broadcastTransaction_
*   _transactionBytes_ is the bytecode of a signed transaction (optional)
*   _transactionJSON_ is the transaction object (optional if _transactionBytes_ provided)
*   _prunableAttachmentJSON_ is the attachment object embedded in _transactionJSON_ containing a prunable message (required if _transactionJSON_ not provided because _transactionBytes_ never includes prunable data)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _fullHash_ (S) is the full hash of the signed transaction
*   _transaction_ (S) is the transaction ID

**Example:** Refer to [Broadcast Transaction](API_Examples.md#broadcast-transaction "The Blue0x API Examples") example.

### Calculate Full Hash

Calculate the full hash of a transaction.

**Request:**

*   _requestType_ is _calculateFullHash_
*   _unsignedTransactionJSON_ is the unsigned transaction JSON object (optional)
*   _unsignedTransactionBytes_ are the unsigned bytes of a transaction (optional if _unsignedTransactionJSON_ is provided)
*   _signatureHash_ is a SHA-256 hash of the transaction signature

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _fullHash_ (S) is the full hash of the signed transaction

**Example:** Refer to [Calculate Full Hash](API_Examples.md#calculate-full_Hash "The Blue0x API Examples") example.

### Get Expected Transactions

Returns the non-phased unconfirmed transactions expected to be included in the next block (only), plus the phased transactions scheduled to finish in that block (whether approved or not).

**Request:**

*   _requestType_ is _getExpectedTransactions_
*   _account_ is one account ID (optional)
*   _account_ is one account ID (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _expectedTransactions_ (A) is an array of expected transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Expected Transactions](API_Examples.md#get-expected-transactions "The Blue0x API Examples") example.

### Get Referencing Transactions

Gets the transactions referencing a given transaction id.

**Request:**

*   _requestType_ is _getReferencingTransactions_
*   _transaction_ is one transaction ID
*   _firstIndex_ is a zero-based index to the first block ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Referencing Transactions](API_Examples.md#get-referencing-transactions "The Blue0x API Examples") example.

### Get Transaction

Get a transaction object given a transaction ID.

**Request:**

*   _requestType_ is _getTransaction_
*   _transaction_ is the transaction ID (optional)
*   _fullHash_ is the full hash of the transaction (optional if transaction ID is provided)
*   _includePhasingResult_ is _true_ to retrieve execution status of each phased transaction (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _sender_ (S) is the account ID of the sender
*   _senderRS_ (S) is the Reed-Solomon address of the sender
*   _feeNQT_ (S) is the fee (in NQT) of the transaction
*   _amountNQT_ (S) is the amount (in NQT) of the transaction
*   _transactionIndex_ (N) is a zero-based index giving the order of the transaction in its block
*   _timestamp_ (N) is the time (in seconds since the genesis block) of the transaction
*   _referencedTransactionFullHash_ (S) is the full hash of a transaction referenced by this one, omitted if no previous transaction is referenced
*   _confirmations_ (N) is the number of transaction confirmations
*   _subtype_ (N) is the transaction subtype (refer to [Get Constants](index.md#get-constants "The Blue0x API") for a current list of subtypes)
*   _block_ (S) is the ID of the block containing the transaction
*   _blockTimestamp_ (N) is the timestamp (in seconds since the genesis block) of the block
*   _height_ (N) is the height of the block in the blockchain
*   _senderPublicKey_ (S) is the public key of the sending account for the transaction
*   _type_ (N) is the transaction type (refer to [Get Constants](index.md#get-constants "The Blue0x API") for a current list of types)
*   _deadline_ (N) is the deadline (in minutes) for the transaction to be confirmed
*   _signature_ (S) is the digital signature of the transaction
*   _recipient_ (S) is the account number of the recipient, if applicable
*   _recipientRS_ (S) is the Reed-Solomon address of the recipient, if applicable
*   _fullHash_ (S) is the full hash of the signed transaction
*   _signatureHash_ (S) is a SHA-256 hash of the transaction signature
*   _approved_ (B) is a boolean indicating if the transaction is approved (only included when _includePhasingResult_ is true and the transaction is phased)
*   _result_ (S) is a string containing the result of the transaction (only included when _includePhasingResult_ is true and the transaction is phased)
*   _executionHeight_ (N) is the height the transaction was executed (only included when _includePhasingResult_ is true and the transaction is phased)
*   _transaction_ (S) is the transaction ID
*   _version_ (N) is the transaction version number
*   _phased_ (B) is _true_ if the transaction is phased, _false_ otherwise
*   _ecBlockId_ (N) is the economic clustering block ID
*   _ecBlockHeight_ (N) is the economic clustering block height
*   _attachment_ (O) is an object containing any additional data needed for the transaction, if applicable
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _block_, _blockTimestamp_ and _confirmations_ fields are omitted for unconfirmed transactions. Double-spending transactions are not retrieved.

**Example:** Refer to [Get Transaction](API_Examples.md#get-transaction "The Blue0x API Examples") example.

### Get Transaction Bytes

Get the bytecode of a transaction.

**Request:**

*   _requestType_ is _getTransactionBytes_
*   _transaction_ is a transaction ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _confirmations_ (N) is the number of transaction confirmations
*   _transactionBytes_ (S) are the bytes contained in the transaction
*   _unsignedTransactionBytes_ (S) are the unsigned bytes contained in the transaction
*   _prunableAttachmentJSON_ (O) is the prunable attachment JSON object, if applicable, because _transactionBytes_ never includes prunable data
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Transaction Bytes](API_Examples.md#get-transaction-bytes "The Blue0x API Examples") example.

### Parse Transaction

Get a transaction object given a (signed or unsigned) transaction bytecode, or re-parse a transaction object. Verify the signature.

**Request:**

*   _requestType_ is _parseTransaction_
*   _transactionBytes_ is the signed or unsigned bytecode of the transaction (optional)
*   _transactionJSON_ is the transaction object (optional if transactionBytes is included)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for additional fields.

*   _verify_ (B) is _true_ if the signature is verified, _false_ otherwise

**Example:** Refer to [Parse Transaction](API_Examples.md#parse-transaction "The Blue0x API Examples") example.

### Retrieve Pruned Transaction

Force retrieval of the prunable data for a given transaction, even if past the configured nxt.maxPrunableLifetime.

**Request:**

*   _requestType_ is _retrievePrunedTransaction_
*   _transaction_ is transaction ID

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for fields.

**Example:** Refer to [Retrieve Pruned Transaction](API_Examples.md#retrieve-pruned-transaction "The Blue0x API Examples") example.

### Send Transaction

It broadcasts a transaction to the network without validating it, without re-broadcasting it and without adding it locally as unconfirmed transaction. Specially intended for roaming or light clients to send transactions to remote peers. POST only.

**Request:**

*   _requestType_ is _sendTransaction_
*   _transactionBytes_ is the bytecode of a signed transaction (optional)
*   _transactionJSON_ is the transaction object (optional if _transactionBytes_ provided)
*   _prunableAttachmentJSON_ is the attachment object embedded in _transactionJSON_ containing a prunable message (required if _transactionJSON_ not provided because _transactionBytes_ never includes prunable data)
*   _adminPassword_ is a string with the admin password (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _fullHash_ (S) is the full hash of the signed transaction
*   _transaction_ (S) is the transaction ID

**Example:** Refer to [Send Transaction](API_Examples.md#send-transaction "The Blue0x API Examples") example.

### Sign Transaction

Calculates the full hash, signature hash, and transaction ID of an unsigned transaction.

**Request:**

*   _requestType_ is _signTransaction_
*   _unsignedTransactionJSON_ is the _transactionJSON_ field of the transaction, without a signature subfield
*   _unsignedTransactionBytes_ is the _unsignedTransactionBytes_ field of the transaction (optional, if _unsignedTransactionJSON_ provided)
*   _prunableAttachmentJSON_ is a prunable attachment JSON object, if applicable, because _unsignedTransactionBytes_ never includes prunable data (optional if the transaction has already been broadcast and the prunable message can still be retrieved from the database)
*   _secretPhrase_ is the secret passphrase of the signing account
*   _validate_ is _false_ to skip validation of the transaction bytes being signed (useful on nodes where the full blockchain is not downloaded)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _signatureHash_ (S) is a SHA-256 hash of the transaction signature, used in escrow transactions
*   _verify_ (B) is _true_ the signature is verified, _false_ if not
*   _transactionJSON_ (O) is the signed transaction JSON object
*   _transactionBytes_ (S) are the signed transaction bytes
*   _fullHash_ (S) is the full hash of the signed transaction
*   _prunableAttachmentJSON_ (O) is the prunable attachment JSON object, if applicable, because _transactionBytes_ never includes prunable data
*   _transaction_ (S) is the transaction ID, derived from the _fullHash_
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Sign Transaction](API_Examples.md#sign-transaction "The Blue0x API Examples") example.