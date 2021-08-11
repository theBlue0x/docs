Server Information Operations
--------------------------------

### Event Register

Create, modify or remove an event listener which can report server events via [Event Wait](#event_Wait "The Blue0x API"). POST only.

**Request:**

*   _requestType_ is _eventRegister_
*   _event_ is one of multiple server events from the following list of event names: (optional, default is all possible events)
    *   Block.BLOCK\-GENERATED
    *   Block.BLOCK\-POPPED
    *   Block.BLOCK\-PUSHED
    *   Peer.ADD\-INBOUND
    *   Peer.ADDED\-ACTIVE\-PEER
    *   Peer.BLACKLIST
    *   Peer.CHANGED\-ACTIVE\-PEER
    *   Peer.DEACTIVATE
    *   Peer.NEW\-PEER
    *   Peer.REMOVE
    *   Peer.REMOVE\-INBOUND
    *   Peer.UNBLACKLIST
    *   Transaction.ADDED\-CONFIRMED\-TRANSACTIONS
    *   Transaction.ADDED\-UNCONFIRMED\-TRANSACTIONS
    *   Transaction.REJECT\-PHASED\-TRANSACTION
    *   Transaction.RELEASE\-PHASED\-TRANSACTION
    *   Transaction.REMOVE\-UNCONFIRMED\-TRANSACTIONS
*   _event_ is one of multiple server events (optional)
*   _add_ is _true_ to add events to an existing listener (optional, omit if _remove_ is _true_)
*   _remove_ is _true_ to remove events from an existing listener (optional, omit if _add_ is _true_)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** To create a new event listener, omit both _add_ and _remove_. To remove an existing event listener, set _remove_ to _true_ and omit _event_; all registered events will be removed, any outstanding [Event Wait](#event_Wait "The Blue0x API") will be completed and the listener will be deactivated.

**Note:** An event listener is automatically deactivated whenever all registered events are removed or if [Event Wait](#event_Wait "The Blue0x API") is not called frequently enough, according to the _nxt.apiEventTimeout_ property. The timeout is not precise; the removal process runs every _nxt.apiEventTimeout_ / 2 seconds, so that many extra seconds may elapse before removal; the first [Event Wait](#event_Wait "The Blue0x API") call should be made immediately after registration to avoid deactivation.

**Note:** Each API user (with a unique address) can create only one event listener. When a new one is created, it will replace an existing one. The maximum number of unique users is controlled by the _nxt.maxEventUsers_ property.

**Response:**

*   _registered_ is _true_ if the operation completed successfully
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Event Register](API_Examples.md#event-register "The Blue0x API Examples") example.

### Event Wait

Wait for events registered with [Event Register](#event-register "The Blue0x API"). POST only.

**Request:**

*   _requestType_ is _eventWait_
*   _timeout_ is the amount of time (in seconds) to wait for an event before the call returns (optional, default and maximum is the nxt.apiEventTimeout property)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Notes:** The call returns immediately if one or more events have occurred since the last call; multiple events are all returned together. If a new call is made before the last one returns, the _timeout_ timer resets to the new value. Event registration expires if wait calls are not made frequently enough, according to the _nxt.apiEventTimeout_ property.

**Response:**

*   _events_ (A) is an array of event objects each of which has the following fields:
    *   _name_ (S) is the name of the event (refer to [Event Register](#event-register "The Blue0x API") for the list of event names)
    *   _ids_ (A) is an array of identifiers, depending on the type of event:
        *   block string identifier (S) for a block event
        *   peer network address (S) for a peer event
        *   transaction string identifier (S) for a transaction event
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Event Wait](API_Examples.md#event-wait "The Blue0x API Examples") example.

### Get Blockchain Status

Get the blockchain status.

**Request:**

*   _requestType_ is _getBlockchainStatus_

**Response:**

*   _currentMinRollbackHeight_ (N) is the current minimum rollback height
*   _numberOfBlocks_ (N) is the number of blocks in the blockchain (height + 1)
*   _isTestnet_ (B) is _true_ if the node is connected to testnet, _false_ otherwise
*   _includeExpiredPrunable_ (B) is the value of the _nxt.includeExpiredPrunable_ property
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _version_ (S) is the application version
*   _maxRollback_ (N) is the value of the _nxt.maxRollback_ property
*   _lastBlock_ (S) is the last block ID on the blockchain
*   _application_ (S) is application name, typically _NRS_
*   _isScanning_ (B) is _true_ if the blockchain is being scanned by the application, _false_ otherwise
*   _isDownloading_ (B) is _true_ if a download is in progress, _false_ otherwise; _true_ when a batch of more than 10 blocks at once has been downloaded from a peer, reset to _false_ when an attempt to download more blocks from a peer does not result in any new blocks
*   _cumulativeDifficulty_ (S) is the cumulative difficulty
*   _lastBlockchainFeederHeight_ (N) is the height of the last blockchain of greatest cumulative difficulty obtained from a peer
*   _maxPrunableLifetime_ (N) is the maximum prunable lifetime (in seconds)
*   _time_ (N) is the current timestamp (in seconds since the genesis block)
*   _lastBlockchainFeeder_ (S) is the address or announced address of the peer providing the last blockchain of greatest cumulative difficulty
*   _blockchainState_ (S) Current state of this node's blockchain (UP\-TO\-DATE or DOWNLOADING)

**Example:** Refer to [Get Blockchain Status](API_Examples.md#get-blockchain-status "The Blue0x API Examples") example.

### Get Constants

Get all defined constants.

**Request:**

*   _requestType_ is _getConstants_

**Response:**

*   _maxBlockPayloadLength_ (N) is the maximum block payload length (in bytes)
*   _maxArbitraryMessageLength_ (N) is the maximum length (in bytes) of an arbitrary message
*   _maxPrunableMessageLength_ (N) is the maximum length (in bytes) of a prunable message
*   _maxTaggedDataDataLength_ (N) is the maximum length (in bytes) of tagged data
*   _maxPhasingDuration_ (N) is the maximum allowed phasing duration in block height
*   _epochBeginning_ (N) is the time in milliseconds when genesis block was created
*   _genesisAccountId_ (S) is the genesis account number
*   _genesisBlockId_ (S) is the genesis block ID
*   _transactionTypes_ (A) is an array of defined transaction types and subtypes (refer to the example below)
*   _transactionSubTypes_ (A) is an array of defined transaction subtypes and subtypes (refer to the example below)
*   _peerStates_ (A) is an array of defined peer states (refer to the example below)
*   _currencyTypes_ (A) is an array of defined currency types (refer to the example below)
*   _disabledAPIs_ (A) is an array of configured disabled apis (refer to the example below)
*   _apiTags_ (A) is an array of defined api tags (refer to the example below)
*   _disabledAPITags_ (A) is an array of configured disabled api tags (refer to the example below)
*   _votingModels_ (A) is an array of defined voting models (refer to the example below)
*   _holdingTypes_ (A) is an array of defined holding types (refer to the example below)
*   _minBalanceModels_ (A) is an array of defined minimum balance models (refer to the example below)
*   _shufflingStages_ (A) is an array of defined shuffling stages (refer to the example below)
*   _shufflingParticipantStates_ (A) is an array of defined shuffling participant states (refer to the example below)
*   _hashAlgorithms_ (A) is an array of defined hash algorithms (refer to the example below)
*   _mintingHashAlgorithms_ (A) is an array of defined minting hash algorithms (refer to the example below)
*   _phasingHashAlgorithms_ (A) is an array of defined phasing hash algorithms (refer to the example below)
*   _requestTypes_ (A) is an array of decined request types (refer to the example below)


### Get Plugins

Get a list of all installed plugins on the server.

**Request:**

*   _requestType_ is _getPlugins_

**Response:**

*   _plugins_ (A) is an array of plugin names (S)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Plugins](API_Examples.md#get-plugins "The Blue0x API Examples") example.

### Get State

Get the state of the server node and network.

**Request:**

*   _requestType_ is _getState_
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional); password protected like the [Debug Operations](debug.md "The Blue0x API") if _true_.

**Response:**

*   _numberOfPeers_ (N) is the number of known peers on the network
*   _numberOfGoods_ (N) is the number of DGS goods in the blockchain
*   _numberOfPolls_ (N) is the number of polls in the blockchain
*   _numberOfUnlockedAccounts_ (N) is the number of unlocked accounts on this node
*   _numberOfTransfers_ (N) is the number of AE transfers in the blockchain
*   _includeExpiredPrunable_ (B) is the value of the _nxt.includeExpiredPrunable_ property
*   _numberOfOrders_ (N) is the number of AE orders in the blockchain
*   _numberOfTransactions_ (N) is the number of transactions in the blockchain
*   _maxMemory_ (N) is the maximum amount of memory the node may use (in Bytes)
*   _maxRollback_ (N) is the value of the _nxt.maxRollback_ property
*   _numberOfOffers_ (N) is the number of buy currency offers in the blockchain
*   _isDownloading_ (B) is _true_ if a download is in progress, _false_ otherwise; _true_ when a batch of more than 10 blocks at once has been downloaded from a peer, reset to _false_ when an attempt to download more blocks from a peer does not result in any new blocks
*   _isScanning_ (B) is _true_ if this node is scanning the blockchain, _false_ otherwise
*   _cumulativeDifficulty_ (S) is the current cumulative forging difficulty
*   _numberOfCurrencies_ (N) is the number of currencies in the blockchain
*   _numberOfAssets_ (N) is the number of AE assets in the blockchain
*   _numberOfPrunableMessages_ (N) is the number of prunable messages in the blockchain
*   _freeMemory_ (N) is the amount of free memory on this node (in Bytes)
*   _peerPort_ (N) is the port used for connecting to peers
*   _numberOfVotes_ (N) is the number of votes in the blockchain
*   _availableProcessors_ (N) is the number of processors on this node
*   _numberOfTaggedData_ (N) is the number of tagged data in the blockchain
*   _numberOfActiveAccountLeases_ (N) is the number of active account leases in the blockchain
*   _numberOfAccountLeases_ (N) is the total number of account leases including scheduled leases (first scheduled lease only) in the blockchain
*   _numberOfAccounts_ (N) is the number of accounts in the blockchain
*   _numberOfDataTags_ (N) is the number of data tags in the blockchain
*   _needsAdminPassword_ (B) is _true_ if the _nxt.disableAdminPassword_ property is _false_
*   _currentMinRollbackHeight_ (N) is the current minimum rollback height
*   _numberOfBlocks_ (N) is the number of blocks (height + 1) in the blockchain
*   _isTestnet_ (B) is _true_ if the node is connected to testnet, _false_ otherwise
*   _numberOfCurrencyTransfers_ (N) is the number of currency transfers in the blockchain
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _numberOfPhasedTransactions_ (N) is the number of phased transactions in the blockchain
*   _version_ (S) is the software version on this node
*   _numberOfBidOrders_ (N) is the number of AE bid orders in the blockchain
*   _lastBlock_ (S) is the last block address
*   _totalMemory_ (N) is the amount of memory this node is using (in Bytes)
*   _application_ (S) is the name of the software running on this node (typically _NRS_)
*   _numberOfAliases_ (N) is the number of aliases in the blockchain
*   _numberOfActivePeers_ (N) is the number of active peers on the network
*   _lastBlockchainFeederHeight_ (N) is the height of the last blockchain feeder
*   _maxPrunableLifetime_ (N) is the maximum prunable lifetime (in seconds)
*   _numberOfExchanges_ (N) is the number of currency exchanges in the blockchain
*   _numberOfTrades_ (N) is the number of AE trades in the blockchain
*   _numberOfPurchases_ (N) is the number of DGS purchases in the blockchain
*   _numberOfTags_ (N) is the number of DGS tags in the blockchain
*   _time_ (N) is the current node time (in seconds since the genesis block)
*   _numberOfAskOrders_ (N) is the number of AE ask orders in the blockchain
*   _lastBlockchainFeeder_ (S) is the announced name of the feeder of the last blockchain
*   _isOffline_ (B) is _true_ if this node is connected to other peers, _false_ otherwise

**Note:** AE is Asset Exchange, DGS is Digital Goods Store

**Example:** Refer to [Get State](API_Examples.md#get-state "The Blue0x API Examples") example.

### Get Time

Get the current time.

**Request:**

*   _requestType_ is _getTime_

**Response:**

*   _time_ (N) is the current time (in seconds since the genesis block).
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Time](API_Examples.md#get-time "The Blue0x API Examples") example.