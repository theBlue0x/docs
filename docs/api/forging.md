Forging Operations
---------------------

### Start / Stop / Get Forging

Start or stop forging with an account, or check to see if an account is forging. POST only.

**Request:**

*   _requestType_ is either _startForging_, _stopForging_ or _getForging_
*   _secretPhrase_ is the secret passphrase of the account (optional for _stopForging_ and _getForging_ if password protected like the [Debug Operations](debug.md "The Blue0x API"))

**Response:**

*   _deadline_ (N) is the estimated time (in seconds since the last block) until the account will forge a block (_startForging_ and _getForging_ only)
*   _hitTime_ (N) is the estimated time (in seconds since the genesis block) when the account will forge a block (_startForging_ and _getForging_ only)
*   _remaining_ (N) is the deadline less the elapsed time since the last block (_getForging_ only)
*   _foundAndStopped_ (B) is _true_ if forging was stopped, _false_ if forging was already stopped (_stopForging_ only)
*   _account_ (S) is the account number (_getForging_ only)
*   _accountRS_ (S) is the Reed-Solomon address of the account (_getForging_ only)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** A _getForging_ request returns _errorCode_ 5 if the account is not forging. If the account has a zero _effectiveBalance_, forging can be started but _deadline_, _remainingTime_ and _hitTime_ will be set to zero.

**Example:** Refer to [Start / Stop / Get Forging](API_Examples.md#start-stop-get-forging "The Blue0x API Examples") example.

#### Get Forging

Refer to [Start / Stop / Get Forging](#start-stop-get-forging "The Blue0x API").

#### Start Forging

Refer to [Start / Stop / Get Forging](#start-stop-get-forging "The Blue0x API").

#### Stop Forging

Refer to [Start / Stop / Get Forging](#start-stop-get-forging "The Blue0x API").

### Lease Balance

Lease the entire guaranteed balance of Blue0x to another account, after 1440 confirmations. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _leaseBalance_
*   _period_ is the lease period (in number of blocks, 1440 minimum)
*   _recipient_ is the lessee (recipient) account
*   _recipientPublicKey_ is the public key of the lessee (recipient) account (optional, enhances security of a new account)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Lease Balance](API_Examples.md#lease-balance "The Blue0x API Examples") example.

### Get Next Block Generators

Returns the next block generators ordered by hit time. The list of currently active forgers is first initialized using the block generators with at least 2 blocks generated within the previous 10,000 blocks, excluding accounts without a public key. The list is updated as new blocks are processed. The results are not 100% correct since previously active generators may no longer be running and new generators won't be known until they generate a block.

**Request:**

*   _requestType_ is _getNextBlockGenerators_
*   -limit_ (N) is the number of next block generators to display.

**Response:**

*   _activeCount_ (N) is the number of active forging accounts
*   _lastBlock_ (S) is the last block ID on the blockchain
*   _generators_ (A) is an array containing the number of next block generators requested
    *   _effectiveBalanceNXT_ (N) is the balance (in BLX) of the account available for forging: the unleased guaranteedBalance of this account plus the leased guaranteedBalance of all lessors to this account
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _deadline_ (N) is the estimated time (in seconds since the last block) until the account will forge a block
    *   _account_ (S) is the account number
    *   _hitTime_ (N) is the estimated time (in seconds since the genesis block) when the account will forge a block
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) when the request was executed
*   _height_ (N) is the height of the blockchain

**Example:** Refer to [Get Next Block Generators](API_Examples.md#get_Next-block-generators "The Blue0x API Examples") example.