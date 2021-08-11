Shuffling Operations
-----------------------

Coin shuffling can be used to perform mixing of BLX, MS currencies (unless created as non-shuffleable), or AE assets. Any account can create a new shuffling, specifying the holding to be shuffled, the shuffle amount, number of participants required, and registration deadline.

### Get Account Shufflings

Retrieves info about shufflings for a specific account.

**Request:**

*   _requestType_ is _getAccountShufflings_
*   _account_ is the account ID
*   _includeFinished_ is _true_ to include completed shufflings (optional)
*   _includeHoldingInfo_ is _true_ to include holding info (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _adminPassword_ is a string with the admin password (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _shufflings_ (A) is an array containing the shuffling object (refer to [Get Shuffling](#get-shuffling "The Blue0x API"))

**Example:** Refer to [Get Account Shufflings](API_Examples.md#get-account-shufflings "The Blue0x API Examples") example.

### Get All Shufflings

Retrieves info about all shufflings.

**Request:**

*   _requestType_ is _getAllShufflings_
*   _includeFinished_ is _true_ to include completed shufflings (optional)
*   _includeHoldingInfo_ is _true_ to include holding info (optional)
*   _finishedOnly_ is _true_ to omit not yet finished shufflings (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _adminPassword_ is a string with the admin password (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _shufflings_ (A) is an array containing the shuffling object (refer to [Get Shuffling](#get-shuffling "The Blue0x API"))

**Example:** Refer to [Get All Shufflings](API_Examples.md#get-all-shufflings "The Blue0x API Examples") example.

### Get Assigned Shufflings

Retrieves info about a shufflings that are currently assigned to a specific account.

**Request:**

*   _requestType_ is _getAssignedShufflings_
*   _account_ is the account ID
*   _includeHoldingInfo_ is _true_ to include holding info (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _adminPassword_ is a string with the admin password (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _shufflings_ (A) is an array containing the shuffling object (refer to [Get Shuffling](#get-shuffling "The Blue0x API"))

**Example:** Refer to [Get Assigned Shufflings](API_Examples.md#get-assigned-shufflings "The Blue0x API Examples") example.

### Get Holding Shufflings

Retrieves info about shufflings for a specific holding and/or stage.

**Request:**

*   _requestType_ is _getHoldingShufflings_
*   _holding_ is the holding ID (optional)
*   _stage_ is the stage of the shuffling (See [Get Constants](server.md#get-constants "The Blue0x API") for type definitions) (optional)
*   _includeFinished_ is _true_ to include completed shufflings (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _adminPassword_ is a string with the admin password (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _shufflings_ (A) is an array containing the shuffling object (refer to [Get Shuffling](#get-shuffling "The Blue0x API"))

**Example:** Refer to [Get Holding Shufflings](API_Examples.md#get_Holding-shufflings "The Blue0x API Examples") example.

### Get Shufflers

Retrieves info about active shufflers on the current node.

**Request:**

*   _requestType_ is _getShufflers_
*   _account_ is account ID (optional)
*   _shufflingFullHash_ is shuffling full hash (optional)
*   _secretPhrase_ is secret phrase of the account doing the shuffling (required if adminPassword is not provided)
*   _adminPassword_ is the admin password (required if secretPhrase is not provided)
*   _includeParticipantState_ to include each shuffling participant's state (optional)

**Response:**

*   _shufflers_ (A) is an array containing all currently running shuffling processes on the node.
    *   _account_ (S) is account ID
    *   _accountRS_ (S) is the account Reed Solomon address
    *   _recipient_ (S) is the recipient account ID to where the funds will be sent once the shuffling is completed
    *   _recipientRS_ (S) is the recipient account Reed Solomon address to where the funds will be sent once the shuffling is completed
    *   _shuffling_ (S) is the shuffling ID
    *   _shufflingFullHash_ (S) is the shuffling full hash
    *   _participantState_ (N) is the state for the participant (For more info, see shufflingParticipantStates array in [Get Constants](server.md#get-constants "The Blue0x API"))
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Shufflers](API_Examples.md#get-shufflers "The Blue0x API Examples") example.

### Get Shuffling

Retrieves info about a shuffling.

**Request:**

*   _requestType_ is _getShuffling_
*   _shuffling_ is the shuffling ID
*   _includeHoldingInfo_ is _true_ to include holding info (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blocksRemaining_ (N) number of blocks remaining until current stage is finished.
*   _amount_ (S) the amount of _holdingType_ to be shuffled
*   _shufflingStateHash_ (S) state hash of the shuffling
*   _shufflingFullHash_ (S) the full hash of the shuffling
*   _issuer_ (S) is the issuer account ID
*   _issuerRS_ (S) is the Reed-Solomon address of the issuer account
*   _assignee_ (S) is the current assignee account ID
*   _assigneeRS_ (S) is the Reed-Solomon address of the current assignee account
*   _shuffling_ (S) is the shuffling ID
*   _holding_ (S) is the asset or currency ID
*   _holdingType_ (N) is the holding type (See [Get Constants](server.md#get-constants "The Blue0x API") for type definitions)
*   _stage_ (N) is the current stage of the shuffling (See [Get Constants](server.md#get-constants "The Blue0x API") for type definitions)
*   _participantCount_ (N) is the number of participants required to start the shuffling
*   _registrantCount_ (N) is the number of registrered participants
*   _recipientPublicKeys_ (A) is an array of recipient public keys
*   _holdingInfo_ (A) is an array containing the asset or currency info (if _includeHoldingInfo_ is _true_ and holdingType is not Blue0x)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Shuffling](API_Examples.md#get-shuffling "The Blue0x API Examples") example.

### Get Shuffling Participants

Retrieves info about participants in a shuffling.

**Request:**

*   _requestType_ is _getShufflingParticipants_
*   _shuffling_ is the shuffling ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _participants_ (A)
    *   _shuffling_ (S) is the shuffling ID
    *   _account_ (S) is the account ID
    *   _accountRS_ (S) is the account Reed Solomong address
    *   _state_ (N) is the state for the participant (For more info, see shufflingParticipantStates array in [Get Constants](server.md#get-constants "The Blue0x API"))
    *   _nextAccount_ (S) is the account ID of the next account in turn
    *   _nextAccountRS_ (S) is the account Reed Solomon address of the next account in turn
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Shuffling Participants](API_Examples.md#get-shuffling-participants "The Blue0x API Examples") example.

### Shuffling Cancel

Cancels a shuffling

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingCancel_
*   _shuffling_ is the shuffling ID
*   _shufflingStateHash_ is the state hash of the shuffling
*   _cancellingAccount_ is the account ID (optional)

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

### Shuffling Create

Creates a new shuffling.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingCreate_
*   _holding_ is the holding id (optional if holdingType is 0)
*   _holdingType_ is the holding type (See [Get Constants](server.md#get-constants "The Blue0x API") for type definitions) (optional)
*   _amount_ is the amount of the holding to shuffle
*   _participantCount_ is the number of participants
*   _registrationPeriod_ is the number of blocks the participants have to register until the shuffle is cancelled

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Create](API_Examples.md#shuffling-create "The Blue0x API Examples") example.

### Shuffling Process

Manually process the shuffling for a specific participant. Note that the shuffling must be in processing stage and the _secretPhrase_ must match the current shuffling assignee.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingProcess_
*   _shuffling_ is the shuffling ID
*   _recipientSecretPhrase_ is the secret phrase of the recipient account (optional if _recipientPublicKey_ is provided)
*   _recipientPublicKey_ is the public key of the recipient account (optional if _recipientSecretPhrase_ is provided)

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Process](API_Examples.md#shuffling-process "The Blue0x API Examples") example.

### Shuffling Register

Registers a new participant to an existing shuffling. The shuffling must be in stage registration.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingRegister_
*   _shufflingFullHash_ is the full hash of the shuffling to register

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Register](API_Examples.md#shuffling-register "The Blue0x API Examples") example.

### Shuffling Verify

Sends a verification that an account's recipient public key is found within a shuffling.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingVerify_
*   _shuffling_ is the shuffling ID
*   _shufflingStateHash_ is the current state hash of the shuffle

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Verify](API_Examples.md#shuffling-verify "The Blue0x API Examples") example.

### Start Shuffler

Starts a automated Shuffler. Once started, the Shuffler monitors the blockchain state for transactions relevant to the specified shuffle, and automatically submits the required transactions on behalf of the user, performing shuffle processing, verification, or cancellation as needed.

**Request:**

*   _requestType_ is _startShuffler_
*   _secretPhrase_ is the secret phrase of the account entering the shuffling
*   _shufflingFullHash_ the full hash of the shuffling
*   _recipientSecretPhrase_ the secret phrase of the recipient account (optional if _recipientPublicKey_ is present)
*   _recipientPublicKey_ the public key of the recipient account (optional if _recipientSecretPhrase_ is present)

**Response**

*   _shuffling_ (S) is the shuffling ID
*   _shufflingFullHash_ (S) is the full hash of the shuffling
*   _account_ (S) is the account ID
*   _accountRS_ (S) is the account Reed Solomong address
*   _recipient_ (S) is the account ID of the recipient account
*   _recipientRS_ (S) is the account Reed Solomon address of the recipient account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Start Shuffler](API_Examples.md#start-shuffler "The Blue0x API Examples") example.

### Stop Shuffler

Stops a previously started automated Shuffler.

**Request:**

*   _requestType_ is _stopShuffler_
*   _account_ is the account ID (optional if _shufflingFullHash_ or _secretPhrase_ or _adminPassword_ is provided)
*   _shufflingFullHash_ the full hash of the shuffling (optional if _account_ or _adminPassword is provided)_
*   _secretPhrase_ is the secret phrase of the account entering the shuffling (optional if _adminPassword_ is provided)
*   _adminPassword_ is the admin password (optional if _secretPhrase_ is provided)

**Response**

*   _stoppedShuffler_ (B) means the specified shuffler was stopped
*   _stoppedAllShufflers_ (B) means all shufflers on the node was stopped (only if _adminPassword_ is provided in request)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Stop Shuffler](API_Examples.md#stop-shuffler "The Blue0x API Examples") example.