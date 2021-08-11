Phasing Operations
---------------------

### Approve Transaction

Approve (vote for) a phased transaction. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _approveTransaction_
*   _transactionFullHash_ is the full hash of the transaction to be approved (may be used up to 10 times per API request)
*   _revealedSecret_ is the secret phrase (required only for _phasingVotingModel 5_, refer to [Create Phasing Poll](#create-phasing-poll "The Blue0x API"))
*   _revealedSecretIsText_ is a way of specifying whether revealedSecret is text or binary.

**Note:** This transaction will be accepted in the blockchain only if all phased transactions it is voting for are already in it.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Approve Transaction](API_Examples.md#approve-transaction "The Blue0x API Examples") example.

### Create Phasing Poll

Create a phased transaction with conditional deferred execution based on the result of a vote, on a list of linked transactions or on the revelation of a secret; or simply with unconditional deferred execution. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is any type from the [Create Transaction](all.md#create-transaction "The Blue0x API") list which is phasing-safe, indicated with italics such as -send Money_
*   _phased_ is _true_ to create a phased transaction (optional but required for all of the following parameters, which are all optional for unphased transactions)
*   _phasingFinishHeight_ is the block height at which the poll will finish; the transaction will be executed at this block height only if all conditions (if any) have been met, otherwise the transaction will never be executed
*   _phasingVotingModel_ is an integer code for the method of approval:
    *   _\-1_ for _None_
    *   _0_ for _Vote By Account_
    *   _1_ for _Vote By Account Balance_
    *   _2_ for _Vote By Asset Balance_
    *   _3_ for _Vote By Currency Balance_
    *   _4_ for -by Linked Transactions_
    *   _5_ for -by Secret_
*   _phasingQuorum_ is the number of "votes" needed for transaction approval (required if _phasingVotingModel_ >= _0_, default _0_):
    *   _0_ for voting model _\-1_
    *   the number of accounts for model _0_
    *   total NQT for model _1_
    *   total QNT for models _2_ and _3_
    *   the number of transactions for model _4_
    *   _1_ for model _5_
*   _phasingMinBalance_ is the minimum balance (in NQT or QNT) required for voting (optional, default _0_)
*   _phasingMinBalanceModel_ is (required if _phasingMinBalance_ > _0_, must match _phasingVotingModel_ when _phasingVotingModel_ = _1_, _2_ or _3_):
    *   _1_ for BLX balance
    *   _2_ for an asset balance
    *   _3_ for a currency balance
*   _phasingHolding_ is the asset or currency ID (required if _phasingMinBalanceModel_ = _2_ or _3_)
*   _phasingWhitelisted_ is the account ID of an account allowed to vote for the transaction; once used, _only_ whitelisted accounts are allowed to vote (optional, may be used up to ten times per API request)
*   _phasingLinkedFullHash_ is the full hash of a transaction that must be in the blockchain at the _phasingFinishHeight_; transactions already in the blockchain before the acceptance of the phased transaction can also be linked, as long as they are not more than 60 days old, or themselves phased transactions (required only for voting model _4_, may be used up to ten times per API request)
*   _phasingHashedSecret_ is the hash of a secret phrase (up to 100 bytes long) required for approval (required only for voting model _5_)
*   _phasingHashedSecretAlgorithm_ is the hash function used: _2_ for SHA256, _6_ for RIPEMD160 and _62_ for SHA256 followed by RIPEMD160, according to [Get Constants](server.md#get-constants "The Blue0x API") (required for a _phasingHashedSecret_)

**Note:** When a balance affects the poll result, the result depends only on the balance (including pending outgoing phased transfers) computed just prior to the finish height.

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Create Phasing Poll](API_Examples.md#create-phasing-poll "The Blue0x API Examples") example.

### Get Account Phased Transaction Count

Get the number of pending phased transactions associated with an account given the account ID.

**Request:**

*   _requestType_ is _getAccountPhasedTransactionCount_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfPhasedTransactions_ (N) is the number of pending phased transactions
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Phased Transaction Count](API_Examples.md#get-account-phased-transaction-count "The Blue0x API Examples") example.

### Get Account Phased Transactions

Get pending phased transactions associated with an account given the account ID in reverse chronological creation order.

**Request:**

*   _requestType_ is _getAccountPhasedTransactions_
*   _account_ is the account ID
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details.

**Example:** Refer to [Get Account Phased Transactions](API_Examples.md#get-account-phased-transactions "The Blue0x API Examples") example.

### Get Asset Phased Transactions

Get pending phased transactions based on an asset in reverse chronological creation order. These transactions can be considered transaction approval requests.

**Request:**

*   _requestType_ is _getAssetPhasedTransactions_
*   _asset_ is the asset ID
*   _account_ is an account ID of the account that created the phased transactions (optional)
*   _withoutWhitelist_ is _true_ to omit phased transactions that include a whitelist (optional)
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details.

**Example:** Refer to [Get Asset Phased Transactions](API_Examples.md#get-asset-phased-transactions "The Blue0x API Examples") example.

### Get Currency Phased Transactions

Get pending phased transactions based on a currency in reverse chronological creation order. These transactions can be considered transaction approval requests.

**Request:**

*   _requestType_ is _getCurrencyPhasedTransactions_
*   _currency_ is the currency ID
*   _account_ is an account ID of the account that created the phased transactions (optional)
*   _withoutWhitelist_ is _true_ to omit phased transactions that include a whitelist (optional)
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details.

**Example:** Refer to [Get Currency Phased Transactions](API_Examples.md#get-currency-phased-transactions "The Blue0x API Examples") example.

### Get Linked Phased Transactions

Gets the phased transactions with by-transaction voting model for a given linkedFullHash, regardless of their phasing status (pending, approved or rejected). Since the corresponding table is trimmed after finish height however, the result will not include those transactions that finished before the last trimming height.

**Request:**

*   _requestType_ is _getLinkedPhasedTransactions_
*   -linkedFullHash_ is the full hash of the transaction transaction
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of transactions (refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Linked Phased Transactions](API_Examples.md#get-linked-phased-transactions "The Blue0x API Examples") example.

### Get Phasing Poll

Get the details of a phasing poll.

**Request:**

*   _requestType_ is _getPhasingPoll_
*   _transaction_ is the transaction ID of the phasing poll
*   _countVotes_ is _true_ to compute the poll _result_ while the votes are still available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transaction_ (S) is the transaction ID of the phasing poll
*   _account_ (S) is the number of the account that created the phasing poll
*   _accountRS_ (S) is the Reed-Solomon address of the account that created the phasing poll
*   _finishHeight_ (N) is the block height at which the poll finished or will finish
*   _votingModel_ (N) is the voting model (refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API"))
*   _quorum_ (S) is the minimum number of votes needed to approve the poll
*   _transactionFullHash_ (S) is the full hash of the phasing poll transaction
*   _finished_ (B) is _true_ if the poll is finished, _false_ otherwise (omitted if _finished_ is _false_)
*   _result_ (S) is the sum of the _result_ of each account that approved (voted for) the transaction; an account's _result_ is _1_ if the voting model is _0_, _4_ or _5_; it is the NQT, asset QNT or currency QNT balance of the account if the voting model is _1_, _2_ or _3_ respectively; however, the _result_ is _0_ if _minBalance_ is not met
*   _approved_ (B) is _true_ if the poll was approved, _false_ otherwise
*   _minBalance_ (S) is the required minimum balance of voting accounts to be eligible to vote
*   _minBalanceModel_ (N) is the minimum balance model (refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API"))
*   _hashedSecret_ (S) is the hash of a secret that must be included in each approval (vote) transaction for the approval to be accepted (refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API"))
*   -linkedFullHashes_ (A) is an array of full hashes of linked transactions (omitted if _votingModel_ != _4_)
*   _whitelist_ (A) is an array of whitelist objects containing the following two fields (omitted if _votingModel_ != _5_):
    *   _whitelisted_ (S) is the number of the whitelisted account
    *   _whitelistedRS_ (S) is the Reed-Solomon address of the whitelisted account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Phasing Poll](API_Examples.md#get-phasing-poll "The Blue0x API Examples") example.

### Get Phasing Poll Vote

Get a cast phasing poll vote given a phased transaction ID and an account ID of a voter, if it is still available.

**Request:**

*   _requestType_ is _getPhasingPollVote_
*   _transaction_ is the phased transaction ID
*   _account_ is the account ID of a voter in the poll
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _voter_ (S) is the account ID of the voter in the poll
*   _voterRS_ (S) is the Reed-Solomon address of the _voter_
*   _transaction_ (S) is the phased transaction ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Phasing Poll Vote](API_Examples.md#get-phasing-poll-vote "The Blue0x API Examples") example.

### Get Phasing Poll Votes

Get all cast phasing poll votes in a phasing poll given a phased transaction ID, if they are still available.

**Request:**

*   _requestType_ is _getPhasingPollVotes_
*   _transaction_ is the phased transaction ID
*   _firstIndex_ is a zero-based index to the first vote to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last vote to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Phasing Poll Vote](#get-phasing-poll-vote "The Blue0x API")

**Example:** Refer to [Get Phasing Poll Votes](API_Examples.md#get-phasing-poll-votes "The Blue0x API Examples") example.

### Get Phasing Polls

Get phasing poll details given multiple phased transaction IDs.

**Request:**

*   _requestType_ is _getPhasingPolls_
*   _transaction_ is one of the multiple phased transaction IDs
*   _transaction_ is one of the multiple phased transaction IDs

⋮

*   _countVotes_ is _true_ to compute the poll _result_ while the votes are still available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Phasing Poll](#get-phasing-poll "The Blue0x API").

**Example:** Refer to [Get Phasing Polls](API_Examples.md#get-phasing-polls "The Blue0x API Examples") example.

### Get Voter Phased Transactions

Get pending phased transactions which include a whitelist in reverse chronological creation order. These transactions can be considered transaction approval requests.

**Request:**

*   _requestType_ is _getVoterPhasedTransactions_
*   _account_ is a whitelisted account ID included in the phased transactions
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Transaction](transactions.md#get-transaction "The Blue0x API") for details.

**Example:** Refer to [Get Voter Phased Transactions](API_Examples.md#get-voter-phased-transactions "The Blue0x API Examples") example.