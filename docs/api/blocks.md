Block Operations
-------------------

### Get Block

Get a block object given a block ID or block height.

**Request:**

*   _requestType_ is _getBlock_
*   _block_ is the block ID (optional)
*   _height_ is the block height (optional if _block_ provided)
*   _timestamp_ is the timestamp (in seconds since the genesis block) of the block (optional if _height_ provided)
*   _includeTransactions_ is _true_ to include transaction details (optional)
*   _includeExecutedPhased_ is _true_ to include approved and executed phased transactions (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** _block_ overrides _height_ which overrides _timestamp_.

**Response:**

*   _previousBlockHash_ (S) is the 32-byte hash of the previous block
*   _payloadLength_ (N) is the length (in bytes) of all transactions included in the block
*   _totalAmountNQT_ (S) is the total amount (in NQT) of the transactions in the block
*   _generationSignature_ (S) is the 32-byte generation signature of the generating account
*   _generator_ (S) is the generating account number
*   _generatorPublicKey_ (S) is the 32-byte public key of the generating account
*   _baseTarget_ (S) is the base target for the next block generation
*   _payloadHash_ (S) is the 32-byte hash of the payload (all transactions)
*   _generatorRS_ (S) is the Reed-Solomon address of the generating account
*   _nextBlock_ (S) is the next block ID
*   _numberOfTransactions_ (N) is the number of transactions in the block
*   _blockSignature_ (S) is the 64-byte block signature
*   _transactions_ (A) is an array of transaction IDs or transaction objects (if _includeTransactions_ provided, refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details)
*   _executedPhasedTransactions_ (A) is an array of transaction IDs or transaction objects (if _includeExecutedPhased_ provided, refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details)
*   _version_ (N) is the block version
*   _totalFeeNQT_ (S) is the total fee (in NQT) of the transactions in the block
*   _previousBlock_ (S) is the previous block ID
*   _cumulativeDifficulty_ (S) is the cumulative difficulty for the next block generation
*   _block_ (S) is the block ID
*   _height_ (N) is the zero-based block height
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the block
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Block](API_Examples.md#get-block "The Blue0x API Examples") example.

### Get Block Id

Get a block ID given a block height.

**Request:**

*   _requestType_ is _getBlockId_
*   _height_ is the block height
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _block_ (S) is the block ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Block Id](API_Examples.md#get-block-id "The Blue0x API Examples") example.

### Get Blocks

Get blocks from the blockchain in reverse block height order.

**Request:**

*   _requestType_ is _getBlocks_
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is first block to retrieve (optional, default is zero or the last block on the blockchain)
*   _lastIndex_ is the last block to retrieve (optional, default is _firstIndex_ + 99)
*   _includeTransactions_ is _true_ to include transaction details (optional)
*   _includeExecutedPhased_ is _true_ to include approved and executed phased transactions (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blocks_ (A) is an array of blocks retrieved (refer to [Get Block](#get-block "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Blocks](API_Examples.md#get-blocks "The Blue0x API Examples") example.

### Get EC Block

Get Economic Cluster block data.

**Request:**

*   _requestType_ is _getECBlock_
*   _timestamp_ is the timestamp (in seconds since the genesis block) of the EC block (optional, default (or zero) is the current timestamp)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If _timestamp_ is more than 15 seconds before the timestamp of the last block on the blockchain, _errorCode_ 4 is returned.

**Response:**

*   _ecBlockHeight_ (N) is the EC block height
*   _ecBlockId_ (S) is the EC block ID
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the EC block
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get EC Block](API_Examples.md#get-ec-block "The Blue0x API Examples") example.