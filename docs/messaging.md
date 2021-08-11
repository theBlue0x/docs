Arbitrary Message System Operations
-------------------------------------

### Decrypt From

Decrypt an AES-encrypted message.

**Request:**

*   _requestType_ is _decryptFrom_
*   _secretPhrase_ is the secret passphrase of the recipient
*   _account_ is the account ID of the recipient
*   _data_ is AES-encrypted data
*   _nonce_ is the unique nonce associated with the encrypted data
*   _decryptedMessageIsText_ is _false_ if the decrypted message is a hex string, otherwise the decrypted message is text (optional)
*   _uncompressDecryptedMessage_ is _false_ to prevent gzip uncompression after decryption (optional)

**Response:**

*   _decryptedMessage_ (S) is the decrypted message
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Decrypt From](API_Examples.md#decrypt-from "The Blue0x API Examples") example.

### Download Prunable Message

Downloadins a prunable message attachments directly as binary data. An optional secretPhrase parameter is supported, to allow decryption and downloading of the encrypted part of the message instead of the plain text part.

**Request:**

*   _requestType_ is _downloadPrunableMessage_
*   _transaction_ is the transaction ID
*   _secretPhrase_ is the secret passphrase used to decrypt the encrypted part of the message (optional)
*   _sharedKey_ is the shared key used to decrypt the message (optional) (see [Get Shared Key](#get-shared-key "The Blue0x API"))
*   _retrieve_ is _true_ to retrieve the message from achival node if needed (optional)
*   _requireBlock_ is theblock ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** The prunable data as a binary file.

**Example:** Refer to [Download Prunable Message](API_Examples.md#download-prunable-message "The Blue0x API Examples") example.

### Encrypt To

Encrypt a message using AES without sending it.

**Request:**

*   _requestType_ is _encryptTo_
*   _secretPhrase_ is the secret passphrase of the sender
*   _recipient_ is the account ID of the recipient
*   _messageToEncrypt_ is either UTF-8 text or a string of hex digits to be compressed and converted into a 1000 byte maximum bytecode then encrypted using AES
*   _messageToEncryptIsText_ is _false_ if the message to encrypt is a hex string, otherwise the message to encrypt is text (optional)
*   _compressMessageToEncrypt_ is _false_ to prevent gzip compression before encryption (optional)

**Response:**

*   _data_ (S) is the AES-encrypted data
*   _nonce_ (S) is a 32-byte pseudorandom nonce
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Encrypt To](API_Examples.md#encrypt-to "The Blue0x API Examples") example.

### Get All Prunable Messages

Get all available prunable messages in reverse block timestamp order.

**Request:**

*   _requestType_ is _getAllPrunableMessages_
*   _timestamp_ is the earliest message (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first prunable message to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last prunable message to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _prunableMessages_ (A) is an array of prunable messages (refer to [Get Prunable Message](#get-prunable-message "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millsec)

**Example:** Refer to [Get All Prunable Messages](API_Examples.md#get-all-prunable-messages "The Blue0x API Examples") example.

### Get Prunable Message

Get the prunable message given a transaction ID, optionally decrypting it if encrypted and if a secretPhrase is provided, if it is still available.

**Request:**

*   _requestType_ is _getPrunableMessage_
*   _transaction_ is the transaction ID
*   _secretPhrase_ is the secret phrase needed for decryption (optional)
*   _sharedKey_ is the shared key used to decrypt the message (optional) (see [Get Shared Key](#get-shared-key "The Blue0x API"))
*   _retrieve_ is _true_ to retrieve pruned data from other nodes if not available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _sender_ (S) is the sender account number
*   _senderRS_ (S) is the Reed-Solomon address of the sender account
*   _recipient_ (S) is the recipient account number
*   _recipientRS_ (S) is the Reed-Solomon address of the recipient account
*   _message_ (S) is the plain message text
*   _messageIsText_ (B) is _true_ if the plain message is text, _false_ if it is a hex string
*   _encryptedMessage_ (O) is the encrypted message object, containing _data_ (S) and _nonce_ (S) fields
*   _encryptedMessageIsText_ (B) is _true_ if the encrypted message is text, _false_ if it is a hex string
*   _decryptedMessage_ (S) is the decrypted and decompressed (if necessary) encrypted message, if applicable and if _secretPhrase_ is provided
*   _isCompressed_ (B) is _true_ if the encrypted message was compressed before encryption, if applicable
*   _transaction_ (S) is the transaction ID
*   _transactionTimestamp_ (N) is the transaction timestamp (in seconds since the genesis block)
*   _blockTimestamp_ (N) is the block timestamp (in seconds since the genesis block)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millsec)

**Example:** Refer to [Get Prunable Message](API_Examples.md#get-prunable-message "The Blue0x API Examples") example.

### Get Prunable Messages

Get all still-available prunable messages given an account id, optionally limiting to only those with another account as recipient or sender, in reverse chronological order.

**Request:**

*   _requestType_ is _getPrunableMessages_
*   _account_ is the account ID
*   _otherAccount_ is another account ID, either sender or recipient, to limit the response
*   _secretPhrase_ is the secret phrase needed for decryption (optional)
*   _timestamp_ is the earliest prunable message (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first prunable message to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last prunable message to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _prunableMessages_ (A) is an array of prunable message objects (refer to [Get Prunable Message](#get-prunable-message "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millsec)

**Example:** Refer to [Get Prunable Messages](API_Examples.md#get-prunable-messages "The Blue0x API Examples") example.

### Get Shared Key

Get the one-time shared key used for encryption of messages.

**Request:**

*   _requestType_ is _getSharedKey_
*   _account_ is the recipient account ID
*   _secretPhrase_ is the secret phrase of the sender
*   _nonce_ is the 32-byte pseudorandom nonce

**Response:**

*   _sharedKey_ (S) is shared key as a hexadecimal string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Shared Key](API_Examples.md#get-shared-key "The Blue0x API Examples") example.

### Read Message

Get a message given a transaction ID.

**Request:**

*   _requestType_ is _readMessage_
*   _transaction_ is the transaction ID of the message
*   _secretPhrase_ is the secret passphrase of the account that received the message (optional)
*   _sharedKey_ is the shared key used to decrypt the message (optional) (see [Get Shared Key](#get-shared-key "The Blue0x API"))
*   _retrieve_ is _true_ to retrieve pruned data from archival nodes (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _messageIsPrunable_ (B) is _true_ if there is a plain message and it is prunable, _false_ if it is not prunable
*   _message_ (S) is the plain message, if applicable
*   _encryptedMessageIsPrunable_ (B) is _true_ if there is an encrypted message and it is prunable, _false_ if it is not prunable
*   _decryptedMessage_ (S) is the decrypted message, if applicable and only if the provided _secretPhrase_ belongs to either the sender or receiver of the transaction
*   _decryptedMessageToSelf_ (S) is the decrypted message sent to self, if applicable and only if the provided _secretPhrase_ belongs to the sender of transaction
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Read Message](API_Examples.md#read-message "The Blue0x API Examples") example.

### Send Message

Create an Arbitrary Message transaction. POST only.

**Request:** Refer to [Create Transaction Request](all.md#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _sendMessage_
*   _recipient_ is the account ID of the recipient (optional)
*   _recipientPublicKey_ is the public key of the receiving account (optional, enhances security of a new account)
*   _message_ is either UTF-8 text or a string of hex digits (perhaps previously encoded using an arbitrary algorithm) to be converted into a bytecode with a maximum length of one kilobyte, 42 kilobytes if prunable (optional)
*   _messageIsText_ is _false_ if the message is a hex string, otherwise the message is text (optional)
*   _messageIsPrunable_ is _true_ if the message is prunable (optional)
*   _messageToEncrypt_ is either UTF-8 text or a string of hex digits to be compressed (unless _compressMessageToEncrypt_ is _false_) and converted into a bytecode with a maximum length of one kilobyte, 42 kilobytes if prunable, then encrypted using AES (optional)
*   _messageToEncryptIsText_ is _false_ if the message to encrypt is a hex string, otherwise the message to encrypt is text (optional)
*   _encryptedMessageData_ is already encrypted data which overrides _messageToEncrypt_ if provided (optional)
*   _encryptedMessageNonce_ is a unique 32-byte number which cannot be reused (optional unless _encryptedMessageData_ is provided)
*   _encryptedMessageIsPrunable_ is _true_ if the encrypted message is prunable (optional)
*   _compressMessageToEncrypt_ is _false_ to prevent gzip compression before encryption (optional)
*   _messageToEncryptToSelf_ is either UTF-8 text or a string of hex digits to be compressed (unless _compressMessageToEncryptToSelf_ is false) and converted into a one kilobyte maximum bytecode then encrypted with AES, then sent to the sending account (optional)
*   _messageToEncryptToSelfIsText_ is _false_ if the message to self-encrypt is a hex string, otherwise the message to encrypt is text (optional)
*   _encryptToSelfMessageData_ is already encrypted data which overrides messageToEncryptToSelf if provided (optional)
*   _encryptToSelfMessageNonce_ is a unique 32-byte number which cannot be reused (optional unless encryptToSelfMessageData is provided)
*   _compressMessageToEncryptToSelf_ is _false_ to prevent gzip compression before encryption (optional)

**Note:** Any combination (including none or all) of the three options plain _message_, _messageToEncrypt_, and _messageToEncryptToSelf_ will be included in the transaction. However, one and only one prunable message may be included in a single transaction if there is not already a message of the same type (either plain or encrypted).

**Note:** The _encryptedMessageData-encryptedMessageNonce_ pair or the _encryptToSelfMessageData-encryptToSelfMessageNonce_ pair can be the output of [Encrypt To](#encrypt-to "The Blue0x API")

**Response:** Refer to [Create Transaction Response](all.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Send Message](API_Examples.md#send-message "The Blue0x API Examples") example.

### Verify Prunable Message

Verify that a prunable message obtained from any source, when hashed, matches the hash of the original prunable message.

**Request:** Refer to [Send Message](#send-message "The Blue0x API") for more details about the following request parameters.

*   _requestType_ is _verifyPrunableMessage_
*   _message_ is the plain message, if applicable (optional)
*   _messageIsText_ is _false_ if the provided plain _message_ is a hex string (optional)
*   _encryptedMessageData_ is the data part of the encrypted data-nonce pair (optional if _message_ provided)
*   _encryptedMessageNonce_ is the nonce part of the encrypted data-nonce pair (required if _encryptedMessageData_ provided)
*   _messageToEncryptIsText_ is _false_ if the encrypted message was a hex string before encryption (optional)
*   _compressMessageToEncrypt_ is _false_ if the encrypted message was not compressed before encryption (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** The hash is computed from the message itself plus its associated flag(s) _isText_ and _isCompressed_ (encrypted only); therefore the flag(s) must be provided for verification if different from the default(s). The original _encryptedMessageData-encryptedMessageNonce_ pair used to compute the original hash must be provided again to recompute the hash for verification of a prunable encrypted message.

**Response:**

*   _version.PrunablePlainMessage_ or _version.PrunableEncryptedMessage_ (N) is _1_, the version number
*   _verify_ (B) is _true_ if the original hash matches the hash computed from the provided values
*   _message_ (S) or _encryptedMessage_ (O) is the prunable plain message or the prunable encrypted message object containing the fields:
    *   _data_ (S)
    *   _nonce_ (B)
    *   _isText_ (B)
    *   _isCompressed_ (B)
*   _messageIsText_ (B) is _true_ if the plain message is text, _false_ if it is a hex string, if applicable
*   _messageHash_ or _encryptedMessageHash_ (S) is the hash
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millsec)

**Example:** Refer to [Verify Prunable Message](API_Examples.md#verify-prunable-message "The Blue0x API Examples") example.