Debug Operations
-------------------

All debug utilities require an _adminPassword_ request parameter. See [Admin Password](overview.md#admin-password "The Blue0x API") for more info.

### Clear Unconfirmed Transactions

Empties the unconfirmed transaction pool. POST only.

**Request:**

*   _requestType_ is _clearUnconfirmedTransactions_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Clear Unconfirmed Transactions](API_Examples.md#clear-unconfirmed-transactions "The Blue0x API Examples") example.

### Dump Peers

Get all active peers, optionally of a certain version or a minimum weight.

**Request:**

*   _requestType_ is _dumpPeers_
*   _version_ is a version filter such as _1.5.11_ (optional)
*   _weight_ is a minimum weight filter such as 1000 (optional)
*   _connect_ is _true_ to force a connection attempt to each known peer first (optional); password protected like the [Debug Operations](debug.md "The Blue0x API") if _true_

**Response:**

*   _peers_ (S) is a string of peer IP addresses or DNS names, separated by semicolons
*   _count_ (N) is the number of peers in the _peers_ string.
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Dump Peers](API_Examples.md#dump-peers "The Blue0x API Examples") example.

### Full Reset

Deletes the entire blockchain. POST only.

**Request:**

*   _requestType_ is _fullReset_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** After successful completion of the reset, a new blockchain will automatically begin downloading.

**Example:** Refer to [Full Reset](API_Examples.md#full-reset "The Blue0x API Examples") example.

### Get All Broadcasted Transactions

Get unconfirmed transactions broadcasted from this node but not yet received back from a peer, if transaction rebroadcasting is enabled.

**Request:**

*   _requestType_ is -getAllBroadcastedTransactions_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of broadcasted unconfirmed transactions not yet received back from a peer (S)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Broadcasted Transactions](API_Examples.md#get-all-broadcasted-transactions "The Blue0x API Examples") example.

### Get All Waiting Transactions

Get unconfirmed transactions temporarily kept in memory during transaction processing.

**Request:**

*   _requestType_ is _getAllWaitingTransactions_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of unconfirmed transactions temporarily kept in memory (S)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Waiting Transactions](API_Examples.md#get-all-waiting-transactions "The Blue0x API Examples") example.

### Get Log

Get up to 100 of the most recent log messages from a memory buffer.

**Request:**

*   _requestType_ is _getLog_
*   _count_ is the number of messages to return (optional, default 100)

**Response:**

*   _messages_ (A) is an array of log messages (S)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Log](API_Examples.md#get-log "The Blue0x API Examples") example.

### Get Stack Traces

Get the stack traces of the currently running threads in reverse _id_ order.

**Request:**

*   _requestType_ is _getStackTraces_
*   _depth_ is the maximum trace depth to retrieve (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   -locks_ (A) is an array of lock objects
*   _threads_ (A) is an array of thread objects with the following fields:
    *   _trace_ (A) is an array of traces (S)
    *   _name_ (S) is the thread name
    *   _id_ (N) is the thread ID
    *   _state_ (S) is the thread state, one of _WAITING_, -tIMED\_WAITING_ and -rUNNABLE_
    *   -locks_ (A) is an array of lock objects with the following fields, if not empty:
        *   _trace_ (S)
        *   _depth_ (N)
        *   _name_ (S)
        *   _hash_ (N)

**Example:** Refer to [Get Stack Traces](API_Examples.md#get-stack-traces "The Blue0x API Examples") example.

### Lucene Reindex

Forces a rebuild of the Lucene search index. POST only.

**Request:**

*   _requestType_ is -luceneReindex_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Lucene Reindex](API_Examples.md#lucene-reindex "The Blue0x API Examples") example.

### Pop Off

Removes specified number of blocks (and associated transactions) from the top of the blockchain. POST only.

**Request:**

*   _requestType_ is _popOff_
*   _numBlocks_ is the number of blocks to pop off the blockchain (optional)
*   _height_ is the new height of the blockchain after popping (optional if _numBlocks_ provided)

**Note:** If table trimming is enabled (default), at most 1440 blocks can be popped off without triggering a full rescan.

**Response:**

*   _blocks_ (A) is an array of the blocks popped off (refer to [Get Block](blocks.md#get-block "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Pop Off](API_Examples.md#pop-off "The Blue0x API Examples") example.

### Rebroadcast Unconfirmed Transactions

Rebroadcast transactions in the unconfirmed pool to peers, until received back or found in the blockchain. Rebroadcasting can be disabled by setting the _nxt.enableTransactionRebroadcasting_ property to _false_. POST only.

**Request:**

*   _requestType_ is -rebroadcastUnconfirmedTransactions_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Rebroadcast Unconfirmed Transactions](API_Examples.md#rebroadcast-unconfirmed-transactions "The Blue0x API Examples") example.

### Requeue Unconfirmed Transactions

Requeue unconfirmed transactions. POST only.

**Request:**

*   _requestType_ is _requeueUnconfirmedTransactions_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Requeue Unconfirmed Transactions](API_Examples.md#requeue-unconfirmed-transactions "The Blue0x API Examples") example.

### Retrieve Pruned Data

Initiates a task of requesting and restoring missing prunable data. POST only.

**Request:**

*   _requestType_ is _retrievePrunedData_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _numberOfPrunedData_ (N) is the number of pruned data available pruned data transactions
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Retrieve Pruned Data](API_Examples.md#retrieve-pruned-data "The Blue0x API Examples") example.

### Scan

Scans the top of the blockchain. POST only.

**Request:**

*   _requestType_ is _scan_
*   _numBlocks_ is the number of blocks to scan at the top of the blockchain (optional)
*   _height_ is the height above which blockchain is to be scanned (optional if _numBlocks_ provided)
*   _validate_ is _true_ if signatures are to be re-verified and blocks and transactions re-validated (optional)

**Note:** The derived object tables are rolled back and rebuilt by rescanning the existing blockchain. A request to rescan more than 1440 blocks when table trimming is enabled will do a full rescan starting from height 0. Rescan status is saved in the database, so that if a rescan is interrupted or fails it will resume on restart.

**Response:**

*   _scanTime_ (N) is the scan time
*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Scan](API_Examples.md#scan "The Blue0x API Examples") example.

### Set Logging

Sets the log level and optionally specifies communication events to be logged, without restarting the server. POST only.

**Request:**

*   _requestType_ is _setLogging_
*   -logLevel_ is one of -eRROR_, _WARN_, -iNFO_ or -dEBUG_ with each level in the list including all of the previous levels (optional, default is -iNFO_)
*   _communicationEvent_ is one of multiple communication (HTTP) events to be logged, from the list: -eXCEPTION_, _HTTP-eRROR_, _HTTP-OK_ (optional)
*   _communicationEvent_ is one of multiple communication events (optional)

⋮

**Note:** The initial log level is set by the _nxt.level_ logging property, currently -fINE_ (equivalent to -dEBUG_).

**Response:**

*   -loggingUpdated_ (B) is _true_ if the operation completed successfully

**Example:** Refer to [Set Logging](API_Examples.md#set-logging "The Blue0x API Examples") example.

### Shutdown

Shutdown the server. POST only.

  
**Request:**

*   _requestType_ is _shutdown_
*   _scan_ is _true_ to truncate the derived tables and schedule a full rescan with validation on the next start (optional)

**Response:**

*   _shutdown_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Shutdown](API_Examples.md#shutdown "The Blue0x API Examples") example.

### Trim Derived Tables

Trigger a derived tables trim, and a prunable tables pruning. POST only.

  
**Request:**

*   _requestType_ is _trimDerivedTables_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Trim Derived Tables](API_Examples.md#trim-derived-tables "The Blue0x API Examples") example.