Hallmark Operations
----------------------

### Decode Hallmark

Decode a node hallmark.

**Request:**

*   _requestType_ is _decodeHallmark_
*   _hallmark_ is the hallmark value

**Response:**

*   _valid_ (B) is _true_ if _host_ is less than 100 characters, _weight_ > 0 and the embedded signature is verified
*   _weight_ (N) is the weight assigned to the hallmark
*   _host_ (S) is the IP address or domain name associated with the hallmark
*   _account_ (S) is the account number associated with the hallmark
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _date_ (S) is the date the hallmark was created, in YYYY-MM-DD format
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Decode Hallmark](API_Examples.md#decode-hallmark "The Blue0x API Examples") example.

### Generate Hallmark

Generates a node hallmark. POST only.

**Request:**

*   _requestType_ is _markHost_
*   _secretPhrase_ is the secret passphrase for the account that will be hallmarked on the node
*   _host_ is the IP address or domain name of the node
*   _weight_ is the weight to assign to the node
*   _date_ is the current date in YYYY-MM-DD format

**Response:**

*   _hallmark_ (S) is the hallmark hex string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Generate Hallmark](API_Examples.md#generate-hallmark "The Blue0x API Examples") example.


