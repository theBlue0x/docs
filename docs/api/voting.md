Voting System Operations
---------------------------

### Cast Vote

Cast a vote on a poll. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _castVote_
*   _poll_ is the poll ID
*   _vote00_ is an integer within the allowed range to vote for option (answer) 0 (optional if _minNumberOfOptions_ met, default is _\-128_)
*   _vote01_ is an integer within the allowed range to vote for option (answer) 1 (optional if _minNumberOfOptions_ met, default is _\-128_)
*   _vote02_ is an integer within the allowed range to vote for option (answer) 2 (optional if _minNumberOfOptions_ met, default is _\-128_)

⋮

**Note:** The allowed vote values are integers between _minRangeValue_ and _maxRangeValue_, inclusive. This range, along with the minimum and maximum number of options that can and must be voted on are specified when the poll is created. Refer to [Create Poll](#create-poll "The Blue0x API").

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Cast Vote](API_Examples.md#cast-vote "The Blue0x API Examples") example.

### Create Poll

Create a new poll. POST only.

**Request:** Refer to [Create Transaction Request](index.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _createPoll_
*   _name_ is the name of the poll
*   _description_ is the description of the poll, or the question to be answered
*   _finishHeight_ is the block height when the poll is completed
*   _votingModel_ is _0_ for -one Vote Per Account_, _1_ for _Vote By BLX Balance_, _2_ for _Vote By Asset Balance_ and _3_ for _Vote By Currency Balance_
*   _minNumberOfOptions_ is the minimum number of options (answers) that must be voted for
*   _maxNumberOfOptions_ is the maximum number of options (answers) that can be voted for
*   _minRangeValue_ is the minimum integer value for an option (answer) (>= _0_)
*   _maxRangeValue_ is the maximum integer value for an option (answer) (>= _minRangeValue_)
*   _minBalance_ is the minimum balance (in NQT or QNT) required for voting (optional, default 0)
*   _minBalanceModel_ is (required if _minBalance_ > _0_, must match _votingModel_ when _votingModel_ > _0_)
    *   _1_ for BLX balance
    *   _2_ for an asset balance
    *   _3_ for a currency balance
*   _holding_ is the asset or currency ID (required if _minBalanceModel_ > _1_)
*   _option00_ is the name of option (answer) 0
*   _option01_ is the name of option (answer) 1 (optional)
*   _option02_ is the name of option (answer) 2 (optional)

**Note:** When a balance affects the poll result, the result depends only on the balance (including pending outgoing phased transfers) computed just prior to the finish height.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the poll ID.

**Example:** Refer to [Create Poll](API_Examples.md#create-poll "The Blue0x API Examples") example.

### Get Poll

Get the details of a poll.

**Request:**

*   _requestType_ is _getPoll_
*   _poll_ is the poll ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _poll_ (S) is the poll ID
*   _account_ (S) is the account number of the poll creator
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _name_ (S) is the name of the poll
*   _description_ (S) is the description of the poll, or the question to be answered
*   _finishHeight_ (N) is the block height when the poll is completed
*   _finished_ (B) is _true_ if the poll is completed, _false_ otherwise
*   _votingModel_ (N) is _0_ for -one Vote Per Account_, _1_ for _Vote By BLX Balance_, _2_ for _Vote By Asset Balance_ and _3_ for _Vote By Currency Balance_
*   _minNumberOfOptions_ (N) is the minimum number of options (answers) that must be voted for
*   _maxNumberOfOptions_ (N) is the maximum number of options (answers) that can be voted for
*   _minBalance_ (S) is the minimum balance (in NQT or QNT) required for voting
*   _minBalanceModel_ (N) is _1_ for BLX balance, _2_ for an asset balance, _3_ for a currency balance when _minBalance_ > 0
*   _holding_ is the asset or currency ID when minBalanceModel > 1
*   _options_ (A) is the array of options (answers)
*   _minRangeValue_ (N) is the minimum integer value for an option (answer)
*   _maxRangeValue_ (N) is the maximum integer value for an option (answer)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Poll](API_Examples.md#get-poll "The Blue0x API Examples") example.

### Get Poll Result

Get the result of a poll.

**Request:**

*   _requestType_ is _getPollResult_
*   _poll_ is the poll ID
*   _votingModel_ (optional, default null)
*   _holding_ (optional, default null)
*   _minBalance_ (optional, default _0_)
*   _minBalanceModel_ (required if _minBalance_ > _0_, must match _votingModel_ when _votingModel_ > _0_)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** The _votingModel_, _holding_, _minBalance_ and _minBalanceModel_ parameters are optional and default to the original values specified when the poll was created (refer to [Create Poll](#create-poll "The Blue0x API")). The original values can only be overridden while the votes are still stored in the database, until 1441 blocks after the poll is completed. If _votingModel_ is specified, _holding_, _minBalance_ and _minBalanceModel_ or the defaults specified above apply, otherwise they are ignored.

**Response:**

*   _poll_ (S) is the poll ID
*   _votingModel_ (N) is the votingModel used to calculate _results_ (refer to Note above)
*   _minBalanceModel_ (N) is the minBalanceModel used to calculate _results_ (refer to Note above)
*   _minBalance_ (S) is the minBalance used to calculate _results_ (refer to Note above)
*   _holding_ (S) is the asset or currency ID if the voting model uses an asset or currency balance to determine _weight_, if applicable (refer to Note above)
*   _decimals_ (N) is the number decimal places used by the asset or currency, if applicable
*   _finished_ (B) is _true_ if the poll is complete, _false_ otherwise
*   _options_ (A) is the array of options (answers) of the poll
*   _results_ (A) is an array of result objects with the following fields for each result:
    *   _weight_ (S) is the sum of the _weight_ of each account that voted for the corresponding option (answer); an account's _weight_ is _1_ if the voting model is _0_, otherwise it is the NQT, asset QNT or currency QNT balance of the account if the voting model is _1_, _2_ or _3_ respectively; however, the _weight_ is _0_ if _minBalance_ is not met
    *   _result_ (S) is the sum over each account that voted for the corresponding option (answer) of: the product of the account's _weight_ and the _rangeValue_ selected when the vote was cast.
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Poll Result](API_Examples.md#get-poll-result "The Blue0x API Examples") example.

### Get Poll Vote

Get a poll vote given a poll ID and an account ID.

**Request:**

*   _requestType_ is _getPollVote_
*   _poll_ is the poll ID
*   _account_ is the account ID
*   _includeWeights_ is _true_ to calculate and return the weight assigned to each vote (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _votes_ (A) is an array of votes, the range values (S) corresponding to each option (answer) with empty strings for non-votes
*   _voter_ (S) is the account number of the voter
*   _voterRS_ (S) is the Reed-Solomon address of the voter
*   _transaction_ (S) is the transaction ID of the vote
*   _weight_ (S) is the weight assigned to each vote (applies if _includeWeights_ is _true_)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Votes are deleted from the database 1441 blocks after the poll is finished. Only aggregate results are kept (refer to [Get Poll Result](#get-poll-result "The Blue0x API")).

**Example:** Refer to [Get Poll Vote](API_Examples.md#get-poll-vote "The Blue0x API Examples") example.

### Get Poll Votes

Get all votes on a poll in reverse chronological order.

**Request:**

*   _requestType_ is _getPollVotes_
*   _poll_ is the poll ID
*   _includeWeights_ is _true_ to calculate and return the weight assigned to each vote (optional)
*   _firstIndex_ is a zero-based index to the first vote to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last vote to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _votes_ (A) is an array of vote objects (refer to [Get Poll Vote](#get-poll-vote "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Votes are deleted from the database 1441 blocks after the poll is finished. Only aggregate results are kept (refer to [Get Poll Result](#get-poll-result "The Blue0x API")).

**Example:** Refer to [Get Poll Votes](API_Examples.md#get-poll-votes "The Blue0x API Examples") example.

### Get Polls

Get poll details in reverse creation order.

**Request:**

*   _requestType_ is _getPolls_
*   _account_ is a creation account ID filter (optional)
*   _timestamp_ is the earliest poll (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first poll to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last poll to retrieve (optional)
*   _includeFinished_ is _true_ to include completed polls (optional)
*   _finishedOnly_ is _true_ to exclude not yet executed, phased transactions (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _polls_ (A) is an array of polls (refer to [Get Poll](#get-poll "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Polls](API_Examples.md#get-polls "The Blue0x API Examples") example.

### Search Polls

Search for poll details given a name/description query string.

**Request:**

*   _requestType_ is _searchPolls_
*   _query_ is a full text query on the poll fields _name_ (S) and _description_ (S) in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview) (optional)
*   _firstIndex_ is a zero-based index to the first poll to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last poll to retrieve (optional)
*   _includeFinished_ is _true_ to include completed polls (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _polls_ (A) is an array of polls (refer to [Get Poll](#get-poll "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Polls](API_Examples.md#search-polls "The Blue0x API Examples") example.