General Notes
---------------

### Genesis Block

Many API requests make reference to the genesis block. FYI, the genesis block's ID is [4777664216118977193](https://localhost:2022/nxt?=%2Fnxt&requestType=getBlock&height=0). Sending messages, selling aliases, and leasing balances to the Genesis account are not allowed.

### Flexible Account IDs

All API requests that require an account ID accept either an account number or the corresponding [Reed-Solomon address](https://nxtdocs.jelurida.com/RS_Address_Format "RS Address Format").

### Quantity Units BLX, NQT and QNT

The Blue0x token, BLX, is used to quantify value within the network and a certain amount of BLX is required, as a fee, for each transaction within the network. This fee goes to the node that forges (generates) the new block containing the transaction that is then accepted into the blockchain.

One billion BLX were created in the [Genesis Block](#genesis-block "The Blue0x API"), and 100,000 BLX were then distributed to NXT owners as per the Jelurida license agreement.  

The Blue0x (BLX) blockchain was created on May 7, 2021 at 12:34:00.

The Blue0x system can be thought of as a network owned by all who posses BLX. In this sense, BLX quantifies ownership of or stake in the system. Stakeholders are entitled to forge blocks and collect transaction fees in proportion to the amount of BLX they possess.

Seperate assets and currencies, such as USDX, are created within the Blue0x network. The amount of these assets and currencies are represented as integers in units of QNT, and are priced in NQT per QNT.

### Creating Unsigned Transactions

All API requests that create a new transaction will accept either a _secretPhrase_ or a _publicKey_ parameter:

*   If _secretPhrase_ is supplied, a transaction is created, signed at the server, and broadcast by the server as usual.
*   If only a _publicKey_ parameter is supplied as a 64-digit (32-byte) hex string, the transaction will be prepared by the server and returned in the JSON response as _transactionJSON_ without a signature. This JSON object along with _secretPhrase_ can then be supplied to [Sign Transaction](transactions.md#sign-transaction "The Blue0x API") as _unsignedTransactionJSON_ and the resulting signed _transactionJSON_ can then be supplied to [Broadcast Transaction](transactions.md#broadcast-transaction "The Blue0x API"). This sequence allows for offline signing of transactions so that _secretPhrase_ never needs to be exposed.
*   _unsignedTransactionBytes_ may be used instead of unsigned _transactionJSON_ when there is no encrypted message. Messages to be encrypted require the _secretPhrase_ for encryption and so cannot be included in _unsignedTransactionBytes_.

### Escrow Operations

All API requests that create a new transaction will accept an optional _referencedTransactionFullHash_ parameter which creates a chained transaction, meaning that the new transaction cannot be confirmed unless the referenced transaction is also confirmed. This feature allows a simple way of transaction escrow:

*   Alice prepares and signs a transaction A, but doesn't broadcast it by setting the _broadcast_ parameter to _false_. She sends to Bob the _unsignedTransactionBytes_, the _fullHash_ of the transaction, and the _signatureHash_. All of those are included in the JSON returned by the API request. (Warning: make sure not to send the signed _transactionBytes_, or the _signature_ itself, as then Bob can just broadcast transaction A himself).
*   Bob prepares, signs and broadcasts transaction B, setting the _referencedTransactionFullHash_ parameter to the _fullHash_ of A provided by Alice. He can verify that this hash indeed belongs to the transaction he expects from Alice, by using [Calculate Full Hash](transactions.md#calculate-full-hash "The Blue0x API"), which takes _unsignedTransactionBytes_ and _signatureHash_ (both of which Alice has also sent to Bob). He can also use [Parse Transaction](transactions.md#parse-transaction "The Blue0x API") to decode the unsigned bytes and inspect all transaction fields.
*   Transaction B is accepted in the unconfirmed transaction pool, but as long as A is still missing, B will not be confirmed, i.e. will not be included in the blockchain. If A is never submitted, B will eventually expire -- so Bob should make sure to set a long enough deadline, such as the maximum of 32767 minutes.
*   Once in the unconfirmed transactions pool, Bob has no way of recalling B back. So now Alice can safely submit her transaction A, by just broadcasting the _signedTransactionBytes_ she got in the first step. Transaction A will get included in the blockchain first, and in the next block Bob's transaction B will also be included.

Note that while the above scheme is good enough for a simple escrow, the blockchain does not enforce that if A is included, B will also be included. It may happen due to forks and blockchain reorganization, that B never gets a chance to be included and expires unconfirmed, while A still remains in the blockchain. However, it is not practically possible for Bob to intentionally cause such chain of events and to prevent B from being confirmed.

### Prunable Data

Prunable data can be removed from the blockchain without affecting the integrity of the blockchain. When a transaction containing prunable data is created, only the 32-byte sha256 hash of the prunable data is included in the _transactionBytes_, not the prunable data itself. The non-prunable signed _transactionBytes_ are used to verify the signature and to generate the transaction's _fullHash_ and ID; when the prunable part of the transaction is removed at a later time, none of these operations are affected.

Prunable data has a predetermined minimum lifetime of two weeks (24 hours on the Testnet) from the timestamp of the transaction. Transactions and blocks received from peer nodes are not accepted if prunable data is missing before this time has elapsed. After this time has elapsed, prunable data is no longer included with transactions and blocks transmitted to peer nodes, and is no longer included in the transaction JSON returned by general-purpose API calls such as Get Transaction; the only way to retrieve it, if still available, is through special-purpose API calls such as Get Prunable Message.

Expired prunable data remains stored in the blockchain until removed at the same time derived tables are trimmed, which occurs automatically every 1000 blocks by default. Use [Trim Derived Tables](debug.md#trim-derived-tables "The Blue0x API") to remove expired prunable data immediately.

Prunable data can be preserved on a node beyond the predetermined minimum lifetime by setting the _nxt.maxPrunableLifetime_ property to a larger value than two weeks or to _-1_ to preserve it indefinitely. To force the node to include such preserved prunable data when transactions and blocks are transmitted to peer nodes, set the _nxt.includeExpiredPrunables_ property to _true_, thus making it an archival node.

Currently, there are two varieties of prunable data in the Blue0x system: prunable [Arbitrary Messages](messaging.md "The Blue0x API") and [Tagged Data](tagged_data.md "The Blue0x API"). Both varities have a maximum prunable data length of 42 kilobytes.

### Properties Files

The behavior of some API calls is affected by property settings loaded from files in the _nxt/conf_ directory during Blue0x server intialization. This directory contains the _nxt-default.properties_ and _logging-default.properties_ files, both of which contain default property settings along with documentation. 

It is recommended not to modify default properties files because they can be overwritten during software updates. Instead, properties in the default files can be overridden by including them in optional _nxt.properties_ and _logging.properties_ files in the same directory. For example, a _nxt.properties_ file can be created with the contents:

nxt.isTestnet=true

This causes the Blue0x server to connect to the Testnet instead of the Mainnet.

### Admin Password

Some API functions take an adminPassword parameter, which must match the nxt.adminPassword property unless the nxt.disableAdminPassword property is set to true or the API server only listens on the localhost interface (when the nxt.apiServerHost property is set to 127.0.0.1).

All [Debug Operations](debug.md "The Blue0x API") require adminPassword since they require some kind of privilege. On some functions adminPassword is used so you can override maximum allowed value for lastIndex parameter, which is set to 100 by default by the nxt.maxAPIRecords property. Giving you the option to retrieve more than objects per request.

### Roaming and Light Client Modes

The remote node to use when in roaming and light client modes is selected randomly, but can be changed manually in the UI, or using the [Set API Proxy Peer](networking.md#set-api-proxy-peer "The Blue0x API") API, or forced to a specific peer using the _nxt.forceAPIProxyServerURL_ property.

Remote nodes can be blacklisted from the UI, or using the [Blacklist API Proxy Peer](networking.md#blacklist-api-proxy-peer "The Blue0x API") API. This blacklisting is independent from peer blacklisting. The API proxy blacklisting period can be set using the _nxt.apiProxyBlacklistingPeriod_ property (default 1800000 milliseconds).

API requests that require sending the secret phrase, shared key, or admin password to the server, for features like forging, shuffling, or running a funding monitor, are disabled when in roaming or light client mode.
