Tagged Data Operations
-------------------------

Tagged data are similar to [prunable](index.md#prunable-data "The Blue0x API") plain messages without a recipient, but with additional searchable metadata fields.

### Download Tagged Data

Download tagged data as a file if it is still available.

**Request:**

*   _requestType_ is _downloadTaggedData_
*   _transaction_ is the transaction ID of the tagged data
*   _retrieve_ is _true_ to retrieve pruned data from other nodes if not available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** The downloaded data file named _nxt_, unless there is an error in the request.

**Example:** Refer to [Download Tagged Data](API_Examples.md#download-tagged-data "The Blue0x API Examples") example.

### Extend Tagged Data

Extend the expiration time of already uploaded tagged data. POST only.

**Request:**

*   _requestType_ is _extendTaggedData_
*   _transaction_ is the transaction ID of the tagged data
*   _data_ is the tagged data (optional)
*   _file_ is the pathname of a data file to upload (optional if _data_ provided)
*   _filename_
*   _name_
*   _description_
*   _tags_
*   _type_
*   _channel_
*   _isText_

**Note:** The _data_ and metadata (_filename_, _name_, _description_, _tags_, _type_, _channel_ and _isText_) parameters can be omitted if the tagged data has not yet expired; otherwise, the tagged data can be restored to the blockchain only if these fields have exactly the same values as when the data was uploaded (refer to [Upload Tagged Data](#upload-tagged-data "The Blue0x API")).

**Note:** Anyone can submit an extension, not only the original uploader. Each extend transaction increases the expiration deadline by two weeks (24 hours on Testnet). Extending an existing tagged data from another account does not change the original submitter account ID by which it is indexed and searchable.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Extend Tagged Data](API_Examples.md#extend-tagged-data "The Blue0x API Examples") example.

### Get Account Tagged Data

Get all available tagged data uploaded by a given account in reverse chronological order.

**Request:**

*   _requestType_ is _getAccountTaggedData_
*   _account_ is the account ID
*   _includeData_ is _true_ to include _data_ (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _data_ (A) is an array of tagged data objects (refer to [Get Tagged Data](#get-tagged-data "The Blue0x API") with _hash_ omitted for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Tagged Data](API_Examples.md#get-account-tagged-data "The Blue0x API Examples") example.

### Get All Tagged Data

Get all available tagged data in reverse chronological order.

**Request:**

*   _requestType_ is _getAllTaggedData_
*   _includeData_ is _true_ to include _data_ (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _data_ (A) is an array of tagged data objects (refer to [Get Tagged Data](#get-tagged-data "The Blue0x API") with _hash_ omitted for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Tagged Data](API_Examples.md#get-all-tagged-data "The Blue0x API Examples") example.

### Get Channel Tagged Data

Get available tagged data by channel, optionally filtered by account, in reverse chronological order.

**Request:**

*   _requestType_ is _getChannelTaggedData_
*   _channel_ is the channel string
*   _account_ is an account ID (optional)
*   _includeData_ is _true_ to include _data_ (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _data_ (A) is an array of tagged data objects (refer to [Get Tagged Data](#get-tagged-data "The Blue0x API") with _hash_ omitted for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Channel Tagged Data](API_Examples.md#get-channel-tagged-data "The Blue0x API Examples") example.

### Get Data Tag Count

Get the total number of distinct available data tags.

**Request:**

*   _requestType_ is _getDataTagCount_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfDataTags_ (N) is the total number of distinct data tags
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Data Tag Count](API_Examples.md#get-data-tag-count "The Blue0x API Examples") example.

### Get Data Tags

Get the distinct tags of all available tagged data, with the number of uses of each tag, in order of number of uses, then alphabetical order.

**Request:**

*   _requestType_ is _getDataTags_
*   _firstIndex_ is a zero-based index to the first tag to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tag to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _tags_ (A) is an array of tag objects including the fields:
    *   _tag_ (S) is a tag word
    *   _count_ (N) is the number of uses of _tag_ among all tagged data
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Data Tags](API_Examples.md#get-data-tags "The Blue0x API Examples") example.

### Get Data Tags Like

Prefix search of available data tags, return in alphabetical order.

**Request:**

*   _requestType_ is _getDataTagsLike_
*   _tagPrefix_ is the prefix to search for (2 character minimum) among all data tags
*   _firstIndex_ is a zero-based index to the first tag to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tag to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _tags_ (A) is an array of tag objects including the fields:
    *   _tag_ (S) is a tag word
    *   _count_ (N) is the number of uses of _tag_ among all tagged data
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Data Tags Like](API_Examples.md#get-data-tags-like "The Blue0x API Examples") example.

### Get Tagged Data

Get available tagged data given a transaction ID.

**Request:**

*   _requestType_ is _getTaggedData_
*   _transaction_ is the transaction ID
*   _includeData_ is _true_ to include _data_ (optional)
*   _retrieve_ is _true_ to retrieve pruned data from other nodes if not available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _data_ (S) is the tagged data
*   _hash_ (S) is the hash of the tagged data
*   _filename_ (S) is the metadata _filename_ field
*   _name_ (S) is the metadata _name_ field
*   _description_ (S) is the metadata _description_ field
*   _tags_ (S) is the metadata _tags_ field
*   _parsedTags_ (A) is an array of tag words (S) parsed from _tags_
*   _type_ (S) is the metadata _type_ field
*   _channel_ (S) is the metadata _channel_ field
*   _isText_ (B) is the metadata _isText_ field
*   _account_ (S) is the number of the account that originally uploaded the tagged data
*   _accountRS_ (S) is the Reed-Solomon address of the uploading account
*   _transaction_ (S) is the transaction ID
*   _transactionTimestamp_ (N) is the transaction timestamp (in seconds since the genesis block)
*   _blockTimestamp_ (N) is the block timestamp (in seconds since the genesis block)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Refer to [Upload Tagged Data](#upload-tagged-data "The Blue0x API") for details about the _data_ and metadata (_filename_, _name_, _description_, _tags_, _type_, _channel_ and _isText_) fields.

**Example:** Refer to [Get Tagged Data](API_Examples.md#get-tagged-data "The Blue0x API Examples") example.

### Get Tagged Data Extend Transactions

Retrieves all tagged data extend transactions for a given tagged data upload transaction.

**Request:**

*   _requestType_ is _getTaggedDataExtendTransactions_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _extendTransactions_ (A) is an array of transactions
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Tagged Data Extend Transactions](API_Examples.md#get-tagged-data-extend-transactions "The Blue0x API Examples") example.

### Search Tagged Data

Full text search on available tagged data name, description and tags; optionally filtered by tag, channel or uploading account; return in reverse relevance order.

**Request:**

*   _requestType_ is _searchTaggedData_
*   _query_ is a full text query on the metadata fields _name_ (S), _description_ (S) and _tags_ (S) in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _tag_ is a word in the _tags_ string (optional)
*   _channel_ is a channel string (optional)
*   _account_ is an account ID (optional)
*   _includeData_ is _true_ to include _data_ (optional)
*   _firstIndex_ is a zero-based index to the first tagged data to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tagged data to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _data_ (A) is an array of tagged data objects (refer to [Get Tagged Data](#get-tagged-data "The Blue0x API") with _hash_ omitted for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Tagged Data](API_Examples.md#search-tagged-data "The Blue0x API Examples") example.

### Upload Tagged Data

Upload and broadcast new tagged data. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _uploadTaggedData_
*   _data_ is the data (optional)
*   _file_ is the pathname of a data file to upload (optional if _data_ provided)
*   _filename_ is a filename to associate with _data_ (optional if _file_ uploaded in which case the uploaded filename is always used)
*   _name_ is the name or title of _data_ (optional if _file_ uploaded in which case the uploaded filename is used, but _name_ takes precedence if provided)
*   _description_ is a description of _data_ (optional)
*   _tags_ is a list of up to 5 words from 3 to 20 characters long and separated by spaces and/or commas, describing the actual content of _data_; for example: _audio,mp3,classical_ (optional)
*   _type_ is the mime type of _data_ such as _torrent_, _pdf_, _doc_, _image_, etc. (optional)
*   _channel_ is a data feed label such as _pirate bay torrents_, _wikileaks_, etc. (optional)
*   _isText_ is _false_ if _data_ is a hex string (optional)

**Note:** The maximum length of _data_ plus all associated metadata is 42 kilobytes. The maximum length of _description_ is 1000 bytes. The maximum length of the other metadata (_name_, _tags_, _type_, _channel_ and _filename_) is 100 bytes each.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Upload Tagged Data](API_Examples.md#upload-tagged-data "The Blue0x API Examples") example.

### Verify Tagged Data

Verify expired tagged data downloaded from another node, against the hash in the blockchain.

**Request:**

*   _requestType_ is _verifyTaggedData_
*   _transaction_ is the transaction ID of the tagged data
*   _data_ is the tagged data (optional)
*   _file_ is the pathname of a data file to upload (optional if _data_ provided)
*   _filename_
*   _name_
*   _description_
*   _tags_
*   _type_
*   _channel_
*   _isText_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** The _data_ and metadata (_filename_, _name_, _description_, _tags_, _type_, _channel_ and _isText_) must have exactly the same values as when the data was uploaded (refer to [Upload Tagged Data](#upload-tagged-data "The Blue0x API")).

**Response:**

*   _verify_ (B) is _true_ if the hash of the provided _data_ and metadata matches the hash in the blockchain
*   _hash_ (S) is the hash of the tagged data
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _version.TaggedDataUpload_ (N) is _1_, the version number

**Note:** This call returns an error if there is a hash mismatch.

**Example:** Refer to [Verify Tagged Data](API_Examples.md#verify-tagged-data "The Blue0x API Examples") example.