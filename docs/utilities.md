Utilities
------------

### Decode QR Code

Decodes a base64-encoded jpeg to a UTF-8 string. POST only.

**Request:**

*   _requestType_ is _decodeQRCode_
*   _qrCodeBase64_ is a base64-encoded jpeg string to be decoded

**Response**

*   _qrCodeData_ (S) is a UTF-8 string containing the decoded data from the base64 string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Decode QR Code](API_Examples.md#decode-qr-code "The Blue0x API Examples") example.

### Detect Mime Type

Gets the mime type of uploaded file or data.

**Request:**

*   _requestType_ is _detectMimeType_
*   _data_ is the data (optional)
*   _file_ is the pathname of a data file to upload (optional if _data_ provided)
*   _filename_ is a filename to associate with _data_ (optional if _file_ uploaded in which case the uploaded filename is always used)
*   _isText_ is _false_ if _data_ is a hex string (optional)

**Response**

*   _type_ (S) is the mime type
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Detect Mime Type](API_Examples.md#detect-mime-type "The Blue0x API Examples") example.

### Encode QR Code

Encodes a UTF-8 string to a base64-encoded jpeg. POST only.

**Request:**

*   _requestType_ is _encodeQRCode_
*   _qrCodeData_ is a UTF-8 text string to be encoded
*   _width_ is the width of the output image (optional)
*   _height_ is the height of the output image (optional)

**Response**

*   _qrCodeBase64_ (S) is a base64 string encoding a jpeg image of the QR code
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Encode QR Code](API_Examples.md#encode-qr-code "The Blue0x API Examples") example.

### Full Hash To Id

Converts a full hash to an ID.

**Request:**

*   _requestType_ is _fullHashToId_
*   _fullHash_ is the full hash 64-digit (32-byte) hex string

**Response:**

*   _stringId_ (S) is the ID corresponding to the hash, in the form of an decimal string
*   -longId_ (S) is the signed long integer (8-bytes) representation of the ID used internally, returned as a string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Full Hash To Id](API_Examples.md#full-hash-to-id "The Blue0x API Examples") example.

### Hash

Calculates the hash of a secret for use in [phased transactions](phasing.md#create-phasing-poll "The Blue0x API") with voting model 5 (_Vote By Secret_).

**Request:**

*   _requestType_ is _hash_
*   _hashAlgorithm_ is the hash function used: _2_ for SHA256, _3_ for SHA3, _5_ for SCRYPT, _6_ for RIPEMD160, _25_ for Keccack25 and _62_ for SHA256 followed by RIPEMD160, according to [Get Constants](server.md#get-constants "The Blue0x API")
*   _secret_ is a secret phrase in text form or hex string form
*   _secretIsText_ is _true_ if _secret_ is text, _false_ if it is a hex string (optional)

**Note:** _secret_ is converted from a hex string to a byte array, which is what the hash algorithm expects, unless _secretIsText_ is _true_, in which case _secret_ is first converted from text to a UTF-8 hex string as by [Hex Convert](#hex-convert "The Blue0x API").

**Response:**

*   _hash_ (S) is the hash of the secret, in the form of a hex string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Hash](API_Examples.md#hash "The Blue0x API Examples") example.

### Hex Convert

Converts a text string into a UTF-8 hex string and if the text input is already a hex string, also into text.

**Request:**

*   _requestType_ is _hexConvert_
*   _string_ is a text string, possibly a hex string

**Response:**

*   _binary_ (S) is the converted UTF-8 hex string
*   _text_ (S) is a text string converted from _string_ if it is a valid UTF-8 hex string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Hex Convert](API_Examples.md#hex-convert "The Blue0x API Examples") example.

### Long Convert

Converts an ID to the signed long integer representation used internally.

**Request:**

*   _requestType_ is -longConvert_
*   _id_ is a numerical ID, in decimal form but equivalent to an 8-byte unsigned integer as produced by SHA-256 hashing

**Response:**

*   _stringId_ (S) is the numerical ID
*   -longId_ (S) is the signed long integer (8-bytes) representation of the ID used internally, returned as a string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)


**Example:** Refer to [Long Convert](API_Examples.md#long-convert "The Blue0x API Examples") example.

### RS Convert

Get both the Reed-Solomon account address and the account number given an account ID.

**Request:**

*   _requestType_ is _rsConvert_
*   _account_ is an account ID (either RS address or number)

**Response:**

*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _account_ (S) is the account number

**Example:** Refer to [RS Convert](API_Examples.md#rs-convert "The Blue0x API Examples") example.