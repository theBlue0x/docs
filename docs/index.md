Blue0x API Documentation
=============

Description
-------------

The Blue0x API interacts with Blue0x nodes using HTTP requests to port 2022. Most HTTP requests can use either the GET or POST methods, but some API calls accept only the POST method for security reasons. Responses are returned as JSON objects.

Each API call is documented below, with definitions given for HTTP request parameters and JSON response fields, followed by an example:

*   The JSON response fields are each followed by one of _s_ for string, _a_ for array, _o_ for object, _N_ for number or _b_ for boolean.
*   In the examples, the Blue0x node is represented as _localhost_ and requests and responses are formatted for easy reading; line breaks and spaces are not actually used except in some parameter values. All requests are in URL format which implies the HTTP GET method. When GET is allowed, the URL can be entered into a browser URL field but proper URL encoding is usually required (e.g., spaces in a parameter value must be replaced by _+_ or _%20_). Otherwise, the URL should be used as a guide to preparing an HTTP POST request using cURL, for example.

All API calls can be viewed and tested at [localhost:2022/test](localhost:2022/test) while a local BLX node is running. For specific API calls, use [localhost:2022/test?requestType=](localhost:2022/test?requestType=)_specificRequestType_.

This document is adapted for Blue0x from the [NXT API Documentation](https://nxtdocs.jelurida.com/API "Nxt API").

Table of Contents
-----------

*   [General Notes](#general-notes)
    *   [Genesis Block](#genesis-block)
    *   [Flexible Account IDs](#flexible-account-ids)
    *   [Quantity Units BLX, NQT and QNT](#quantity-units-blx-nqt-and-qnt)
    *   [Creating Unsigned Transactions](#creating-unsigned-transactions)
    *   [Escrow Operations](#escrow-operations)
    *   [Prunable Data](#prunable-data)
    *   [Properties Files](#properties-files)
    *   [Admin Password](#admin-password)
    *   [Roaming and Light Client Modes](#roaming-and-light-client-modes)
*   [Create Transaction](#create-transaction)
    *   [Create Transaction Request](#create-transaction-request)
    *   [Create Transaction Response](index.md#create-transaction-response)
*   [Account Operations](#account-operations)
    *   [Delete Account Property](#delete-account-property)
    *   [Get Account](#get-account)
    *   [Get Account Block Count](#get-account-block-count)
    *   [Get Account Block Ids](#get-account-block-ids)
    *   [Get Account Blocks](#get-account-blocks)
    *   [Get Account Id](#get-account-id)
    *   [Get Account Ledger](#get-account-ledger)
    *   [Get Account Ledger Entry](#get-account-ledger-entry)
    *   [Get Account Lessors](#get-account-lessors)
    *   [Get Account Properties](#get-account-properties)
    *   [Get Account Public Key](#get-account-public-key)
    *   [Get Balance](#get-balance)
    *   [Get Blockchain Transactions](#get-blockchain-transactions)
    *   [Get Guaranteed Balance](#get-guaranteed-balance)
    *   [Get Unconfirmed Transaction Ids](#get-unconfirmed-transaction-ids)
    *   [Get Unconfirmed Transactions](#get-unconfirmed-transactions)
    *   [Search Accounts](#search-accounts)
    *   [Send Money](#send-money)
        *   [Send BLX](#send-blx)
    *   [Set Account Info](#set-account-info)
    *   [Set Account Property](#set-account-property)
    *   [Start Funding Monitor](#start-funding-monitor)
    *   [Stop Funding Monitor](#stop-funding-monitor)
*   [Account Control Operations](#account-control-operations)
    *   [Get All Phasing Only Controls](#get-all-phasing-only-controls)
    *   [Get Phasing Only Control](#get-phasing-only-control)
    *   [Set Phasing Only Control](#set-phasing-only-control)
*   [Alias Operations](#alias-operations)
    *   [Buy / Sell Alias](#buy-sell-alias)
        *   [Buy Alias](#buy-alias)
        *   [Sell Alias](#sell-alias)
    *   [Set Alias](#set-alias)
    *   [Delete Alias](#delete-alias)
    *   [Get Alias](#get-alias)
    *   [Get Alias Count](#get-alias-count)
    *   [Get Aliases](#get-aliases)
    *   [Get Aliases Like](#get-aliases-like)
*   [Arbitrary Message System Operations](#arbitrary-message-system-operations)
    *   [Decrypt From](#decrypt-from)
    *   [Download Prunable Message](#download-prunable-message)
    *   [Encrypt To](#encrypt-to)
    *   [Get All Prunable Messages](#get-all-prunable-messages)
    *   [Get Prunable Message](#get-prunable-message)
    *   [Get Prunable Messages](#get-prunable-messages)
    *   [Get Shared Key](#get-shared-key)
    *   [Read Message](#read-message)
    *   [Send Message](#send-message)
    *   [Verify Prunable Message](#verify-prunable-message)
*   [Asset Exchange Operations](#asset-exchange-operations)
    *   [Cancel Order](#cancel-order)
        *   [Cancel Ask Order](#cancel-ask-order)
        *   [Cancel Bid Order](#cancel-bid-order)
    *   [Delete Asset Shares](#delete-asset-shares)
    *   [Dividend Payment](#dividend-payment)
    *   [Get Account Asset Count](#get-account-asset-count)
    *   [Get Account Assets](#get-account-assets)
    *   [Get Account Current Order Ids](#get-account-current-order-ids)
        *   [Get Account Current Ask Order Ids](#get-account-current-ask-order-ids)
        *   [Get Account Current Bid Order Ids](#get-account-current-bid-order-ids)
    *   [Get Account Current Orders](#get-account-current-orders)
        *   [Get Account Current Ask Orders](#get-account-current-ask-orders)
        *   [Get Account Current Bid Orders](#get-account-current-bid-orders)
    *   [Get All Assets](#get-all-assets)
    *   [Get All Open Orders](#get-all-open-orders)
        *   [Get All Open Ask Orders](#get-all-open-ask-orders)
        *   [Get All Open Bid Orders](#get-all-open-bid-orders)
    *   [Get All Trades](#get-all-trades)
    *   [Get Asset](#get-asset)
    *   [Get Asset Account Count](#get-asset-account-count)
    *   [Get Asset Accounts](#get-asset-accounts)
    *   [Get Asset Deletes](#get-asset-deletes)
    *   [Get Asset Dividends](#get-asset-dividends)
    *   [Get Asset Ids](#get-asset-ids)
    *   [Get Asset Transfers](#get-asset-transfers)
        *   [Get Expected Asset Transfers](#get-expected-asset-transfers)
    *   [Get Assets](#get-assets)
    *   [Get Assets By Issuer](#get-assets-by-issuer)
    *   [Get Expected Asset Deletes](#get-expected-asset-deletes)
    *   [Get Order](#get-order)
        *   [Get Ask Order](#get-ask-order)
        *   [Get Bid Order](#get-bid-order)
    *   [Get Order Ids](#get-order-ids)
        *   [Get Ask Order Ids](#get-ask-order-ids)
        *   [Get Bid Order Ids](#get-bid-order-ids)
    *   [Get Orders](#get-orders)
        *   [Get Ask Orders](#get-ask-orders)
        *   [Get Bid Orders](#get-bid-orders)
        *   [Get Expected Ask Orders](#get-expected-ask-orders)
        *   [Get Expected Bid Orders](#get-expected-bid-orders)
    *   [Get Expected Order Cancellations](#get-expected-order-cancellations)
    *   [Get Last Trades](#get-last-trades)
    *   [Get Order Trades](#get-order-trades)
    *   [Get Trades](#get-trades)
    *   [Issue Asset](#issue-asset)
    *   [Place Order](#place-order)
        *   [Place Ask Order](#place-ask-order)
        *   [Place Bid Order](#place-bid-order)
    *   [Search Assets](#search-assets)
    *   [Transfer Asset](#transfer-asset)
*   [Block Operations](#block-operations)
    *   [Get Block](#get-block)
    *   [Get Block Id](#get-block-id)
    *   [Get Blocks](#get-blocks)
    *   [Get EC Block](#get-ec-block)
*   [Digital Goods Store Operations](#digital-goods-store-operations)
    *   [DGS Delisting](#dgs-delisting)
    *   [DGS Delivery](#dgs-delivery)
    *   [DGS Feedback](#dgs-feedback)
    *   [DGS Listing](#dgs-listing)
    *   [DGS Price Change](#dgs-price-change)
    *   [DGS Purchase](#dgs-purchase)
    *   [DGS Quantity Change](#dgs-quantity-change)
    *   [DGS Refund](#dgs-refund)
    *   [Get DGS Expired Purchases](#get-dgs-expired-purchases)
    *   [Get DGS Good](#get-dgs-good)
    *   [Get DGS Goods](#get-dgs-goods)
    *   [Get DGS Goods Count](#get-dgs-goods-count)
    *   [Get DGS Goods Purchase Count](#get-dgs-goods-purchase-count)
    *   [Get DGS Goods Purchases](#get-dgs-goods-purchases)
    *   [Get DGS Pending Purchases](#get-dgs-pending-purchases)
    *   [Get DGS Purchase](#get-dgs-purchase)
    *   [Get DGS Purchase Count](#get-dgs-purchase-count)
    *   [Get DGS Purchases](#get-dgs-purchases)
    *   [Get DGS Tag Count](#get-dgs-tag-count)
    *   [Get DGS Tags](#get-dgs-tags)
    *   [Get DGS Tags Like](#get-dgs-tags-like)
    *   [Search DGS Goods](#search-dgs-goods)
*   [Forging Operations](#forging-operations)
    *   [Start / Stop / Get Forging](#start-stop-get-forging)
        *   [Get Forging](#get-forging)
        *   [Start Forging](#start-forging)
        *   [Stop Forging](#stop-forging)
    *   [Lease Balance](#lease-balance)
    *   [Get Next Block Generators](#get-next-block-generators)
*   [Hallmark Operations](#hallmark-operations)
    *   [Decode Hallmark](#decode-hallmark)
    *   [Mark Host](#mark-host)
        *   [Generate Hallmark](#generate-hallmark)
*   [Monetary System Operations](#monetary-system-operations)
    *   [Can Delete Currency](#can-delete-currency)
    *   [Currency Buy / Sell](#currency-buy-sell)
        *   [Currency Buy](#currency-buy)
        *   [Currency Sell](#currency-sell)
    *   [Currency Mint](#currency-mint)
    *   [Currency Reserve Claim](#currency-reserve-claim)
    *   [Currency Reserve Increase](#currency-reserve-increase)
    *   [Delete Currency](#delete-currency)
    *   [Get Account Currencies](#get-account-currencies)
    *   [Get Account Currency Count](#get-account-currency-count)
    *   [Get Account Exchange Requests](#get-account-exchange-requests)
        *   [Get Expected Exchange Requests](#get-expected-exchange-requests)
    *   [Get Funding Monitor](#get-funding-monitor)
    *   [Get All Currencies](#get-all-currencies)
    *   [Get All Exchanges](#get-all-exchanges)
    *   [Get Available To Buy](#get-available-to-buy)
        *   [Get Available To Sell](#get-available-to-sell)
    *   [Get Buy / Sell Offers](#get-buy-sell-offers)
        *   [Get Buy Offers](#get-buy-offers)
        *   [Get Sell Offers](#get-sell-offers)
        *   [Get Expected Buy Offers](#get-expected-buy-offers)
        *   [Get Expected Sell Offers](#get-expected-sell-offers)
    *   [Get Currencies](#get-currencies)
    *   [Get Currencies By Issuer](#get-currencies-by-issuer)
    *   [Get Currency](#get-currency)
    *   [Get Currency Account Count](#get-currency-account-count)
    *   [Get Currency Accounts](#get-currency-accounts)
    *   [Get Currency Founders](#get-currency-founders)
    *   [Get Currency Ids](#get-currency-ids)
    *   [Get Currency Transfers](#get-currency-transfers)
        *   [Get Expected Currency Transfers](#get-expected-currency-transfers)
    *   [Get Exchanges](#get-exchanges)
    *   [Get Exchanges By Exchange Request](#get-exchanges-by-exchange-request)
    *   [Get Exchanges By Offer](#get-exchanges-by-offer)
    *   [Get Last Exchanges](#get-last-exchanges)
    *   [Get Minting Target](#get-minting-target)
    *   [Get Offer](#get-offer)
    *   [Issue Currency](#issue-currency)
    *   [Publish Exchange Offer](#publish-exchange-offer)
    *   [Search Currencies](#search-currencies)
    *   [Transfer Currency](#transfer-currency)
*   [Networking Operations](#networking-operations)
    *   [Add Peer](#add-peer)
    *   [Blacklist API Proxy Peer](#blacklist-api-proxy-peer)
    *   [Blacklist Peer](#blacklist-peer)
    *   [Get Inbound Peers](#get-inbound-peers)
    *   [Get My Info](#get-my-info)
    *   [Get Peer](#get-peer)
    *   [Get Peers](#get-peers)
    *   [Set API Proxy Peer](#set-api-proxy-peer)
*   [Phasing Operations](#phasing-operations)
    *   [Approve Transaction](#approve-transaction)
    *   [Create Phasing Poll](#create-phasing-poll)
    *   [Get Account Phased Transaction Count](#get-account-phased-transaction-count)
    *   [Get Account Phased Transactions](#get-account-phased-transactions)
    *   [Get Asset Phased Transactions](#get-asset-phased-transactions)
    *   [Get Currency Phased Transactions](#get-currency-phased-transactions)
    *   [Get Linked Phased Transactions](#get-linked-phased-transactions)
    *   [Get Phasing Poll](#get-phasing-poll)
    *   [Get Phasing Poll Vote](#get-phasing-poll-vote)
    *   [Get Phasing Poll Votes](#get-phasing-poll-votes)
    *   [Get Phasing Polls](#get-phasing-polls)
    *   [Get Voter Phased Transactions](#get-voter-phased-transactions)
*   [Server Information Operations](#server-information-operations)
    *   [Event Register](#event-register)
    *   [Event Wait](#event-wait)
    *   [Get Blockchain Status](#get-blockchain-status)
    *   [Get Constants](#get-constants)
    *   [Get Plugins](#get-plugins)
    *   [Get State](#get-state)
    *   [Get Time](#get-time)
*   [Shuffling Operations](#shuffling-operations)
    *   [Get Account Shufflings](#get-account-shufflings)
    *   [Get All Shufflings](#get-all-shufflings)
    *   [Get Assigned Shufflings](#get-assigned-shufflings)
    *   [Get Holding Shufflings](#get-holding-shufflings)
    *   [Get Shufflers](#get-shufflers)
    *   [Get Shuffling](#get-shuffling)
    *   [Get Shuffling Participants](#get-shuffling-participants)
    *   [Shuffling Cancel](#shuffling-cancel)
    *   [Shuffling Create](#shuffling-create)
    *   [Shuffling Process](#shuffling-process)
    *   [Shuffling Register](#shuffling-register)
    *   [Shuffling Verify](#shuffling-verify)
    *   [Start Shuffler](#start-shuffler)
    *   [Stop Shuffler](#stop-shuffler)
*   [Tagged Data Operations](#tagged-data-operations)
    *   [Download Tagged Data](#download-tagged-data)
    *   [Extend Tagged Data](#extend-tagged-data)
    *   [Get Account Tagged Data](#get-account-tagged-data)
    *   [Get All Tagged Data](#get-all-tagged-data)
    *   [Get Channel Tagged Data](#get-channel-tagged-data)
    *   [Get Data Tag Count](#get-data-tag-count)
    *   [Get Data Tags](#get-data-tags)
    *   [Get Data Tags Like](#get-data-tags-like)
    *   [Get Tagged Data](#get-tagged-data)
    *   [Get Tagged Data Extend Transactions](#get-tagged-data-extend-transactions)
    *   [Search Tagged Data](#search-tagged-data)
    *   [Upload Tagged Data](#upload-tagged-data)
    *   [Verify Tagged Data](#verify-tagged-data)
*   [Token Operations](#token-operations)
    *   [Decode File Token](#decode-file-token)
    *   [Decode Token](#decode-token)
    *   [Generate File Token](#generate-file-token)
    *   [Generate Token](#generate-token)
*   [Transaction Operations](#transaction-operations)
    *   [Broadcast Transaction](#broadcast-transaction)
    *   [Calculate Full Hash](#calculate-full-hash)
    *   [Get Expected Transactions](#get-expected-transactions)
    *   [Get Referencing Transactions](#get-referencing-transactions)
    *   [Get Transaction](#get-transaction)
    *   [Get Transaction Bytes](#get-transaction-bytes)
    *   [Parse Transaction](#parse-transaction)
    *   [Retrieve Pruned Transaction](#retrieve-pruned-transaction)
    *   [Send Transaction](#send-transaction)
    *   [Sign Transaction](#sign-transaction)
*   [Voting System Operations](#voting-system-operations)
    *   [Cast Vote](#cast-vote)
    *   [Create Poll](#create-poll)
    *   [Get Poll](#get-poll)
    *   [Get Poll Result](#get-poll-result)
    *   [Get Poll Vote](#get-poll-vote)
    *   [Get Poll Votes](#get-poll-votes)
    *   [Get Polls](#get-polls)
    *   [Search Polls](#search-polls)
*   [Utilities](#utilities)
    *   [Decode QR Code](#decode-qr-code)
    *   [Detect Mime Type](#detect-mime-type)
    *   [Encode QR Code](#encode-qr-code)
    *   [Full Hash To Id](#full-hash-to-id)
    *   [Hash](#hash)
    *   [Hex Convert](#hex-convert)
    *   [Long Convert](#long-convert)
    *   [RS Convert](#rs-convert)
*   [Debug Operations](#debug-operations)
    *   [Clear Unconfirmed Transactions](#clear-unconfirmed-transactions)
    *   [Dump Peers](#dump-peers)
    *   [Full Reset](#full-reset)
    *   [Get All Broadcasted Transactions](#get-all-broadcasted-transactions)
    *   [Get All Waiting Transactions](#get-all-waiting-transactions)
    *   [Get Log](#get-log)
    *   [Get Stack Traces](#get-stack-traces)
    *   [Lucene Reindex](#lucene-reindex)
    *   [Pop Off](#pop-off)
    *   [Rebroadcast Unconfirmed Transactions](#rebroadcast-unconfirmed-transactions)
    *   [Requeue Unconfirmed Transactions](#requeue-unconfirmed-transactions)
    *   [Retrieve Pruned Data](#retrieve-pruned-data)
    *   [Scan](#scan)
    *   [Set Logging](#set-logging)
    *   [Shutdown](#shutdown)
    *   [Trim Derived Tables](#trim-derived-tables)

General Notes
---------------

### Genesis Block

Many API requests make reference to the genesis block. FYI, the genesis block's ID is [4777664216118977193](localhost:2022/nxt?=%2Fnxt&requestType=getBlock&height=0). Sending messages, selling aliases, and leasing balances to the Genesis account are not allowed.

### Flexible Account IDs

All API requests that require an account ID accept either an account number or the corresponding [Reed-Solomon address](https://nxtwiki.org/wiki/RS-address-format "RS Address Format").

### Quantity Units BLX, NQT and QNT

The Blue0x token, BLX, is used to quantify value within the network and a certain amount of BLX is required, as a fee, for each transaction within the network. This fee goes to the node that forges (generates) the new block containing the transaction that is then accepted into the blockchain.

One billion BLX were created in the [Genesis Block](#genesis-block "The Blue0x API"), and 100,000 BLX were then distributed to NXT owners as per the Jelurida license agreement.  

The Blue0x (BLX) blockchain was created on May 7, 2021 at 12:34:00.

The Blue0x system can be thought of as a network owned by all who posses BLX. In this sense, BLX quantifies ownership of or stake in the system. Stakeholders are entitled to forge blocks and collect transaction fees in proportion to the amount of BLX they possess.

Seperate assets and currencies, such as USDX, are created within the Blue0x network. The amount of these assets and currencies are represented as integers in units of QNT, and are priced in NQT per QNT.

### Creating Unsigned Transactions

All API requests that create a new transaction will accept either a _secretPhrase_ or a _publicKey_ parameter:

*   If _secretPhrase_ is supplied, a transaction is created, signed at the server, and broadcast by the server as usual.
*   If only a _publicKey_ parameter is supplied as a 64-digit (32-byte) hex string, the transaction will be prepared by the server and returned in the JSON response as _transactionJSON_ without a signature. This JSON object along with _secretPhrase_ can then be supplied to [Sign Transaction](#sign-transaction "The Blue0x API") as _unsignedTransactionJSON_ and the resulting signed _transactionJSON_ can then be supplied to [Broadcast Transaction](#broadcast-transaction "The Blue0x API"). This sequence allows for offline signing of transactions so that _secretPhrase_ never needs to be exposed.
*   _unsignedTransactionBytes_ may be used instead of unsigned _transactionJSON_ when there is no encrypted message. Messages to be encrypted require the _secretPhrase_ for encryption and so cannot be included in _unsignedTransactionBytes_.

### Escrow Operations

All API requests that create a new transaction will accept an optional _referencedTransactionFullHash_ parameter which creates a chained transaction, meaning that the new transaction cannot be confirmed unless the referenced transaction is also confirmed. This feature allows a simple way of transaction escrow:

*   Alice prepares and signs a transaction A, but doesn't broadcast it by setting the _broadcast_ parameter to _false_. She sends to Bob the _unsignedTransactionBytes_, the _fullHash_ of the transaction, and the _signatureHash_. All of those are included in the JSON returned by the API request. (Warning: make sure not to send the signed _transactionBytes_, or the _signature_ itself, as then Bob can just broadcast transaction A himself).
*   Bob prepares, signs and broadcasts transaction B, setting the _referencedTransactionFullHash_ parameter to the _fullHash_ of A provided by Alice. He can verify that this hash indeed belongs to the transaction he expects from Alice, by using [Calculate Full Hash](#calculate-full-hash "The Blue0x API"), which takes _unsignedTransactionBytes_ and _signatureHash_ (both of which Alice has also sent to Bob). He can also use [Parse Transaction](#parse-transaction "The Blue0x API") to decode the unsigned bytes and inspect all transaction fields.
*   Transaction B is accepted in the unconfirmed transaction pool, but as long as A is still missing, B will not be confirmed, i.e. will not be included in the blockchain. If A is never submitted, B will eventually expire -- so Bob should make sure to set a long enough deadline, such as the maximum of 32767 minutes.
*   Once in the unconfirmed transactions pool, Bob has no way of recalling B back. So now Alice can safely submit her transaction A, by just broadcasting the _signedTransactionBytes_ she got in the first step. Transaction A will get included in the blockchain first, and in the next block Bob's transaction B will also be included.

Note that while the above scheme is good enough for a simple escrow, the blockchain does not enforce that if A is included, B will also be included. It may happen due to forks and blockchain reorganization, that B never gets a chance to be included and expires unconfirmed, while A still remains in the blockchain. However, it is not practically possible for Bob to intentionally cause such chain of events and to prevent B from being confirmed.

### Prunable Data

Prunable data can be removed from the blockchain without affecting the integrity of the blockchain. When a transaction containing prunable data is created, only the 32-byte sha256 hash of the prunable data is included in the _transactionBytes_, not the prunable data itself. The non-prunable signed _transactionBytes_ are used to verify the signature and to generate the transaction's _fullHash_ and ID; when the prunable part of the transaction is removed at a later time, none of these operations are affected.

Prunable data has a predetermined minimum lifetime of two weeks (24 hours on the Testnet) from the timestamp of the transaction. Transactions and blocks received from peer nodes are not accepted if prunable data is missing before this time has elapsed. After this time has elapsed, prunable data is no longer included with transactions and blocks transmitted to peer nodes, and is no longer included in the transaction JSON returned by general-purpose API calls such as Get Transaction; the only way to retrieve it, if still available, is through special-purpose API calls such as Get Prunable Message.

Expired prunable data remains stored in the blockchain until removed at the same time derived tables are trimmed, which occurs automatically every 1000 blocks by default. Use [Trim Derived Tables](#trim-derived-tables "The Blue0x API") to remove expired prunable data immediately.

Prunable data can be preserved on a node beyond the predetermined minimum lifetime by setting the _nxt.maxPrunableLifetime_ property to a larger value than two weeks or to _\-1_ to preserve it indefinitely. To force the node to include such preserved prunable data when transactions and blocks are transmitted to peer nodes, set the _nxt.includeExpiredPrunables_ property to _true_, thus making it an archival node.

Currently, there are two varieties of prunable data in the Blue0x system: prunable [Arbitrary Messages](#arbitrary-message-system-operations "The Blue0x API") and [Tagged Data](#tagged-data-operations "The Blue0x API"). Both varities have a maximum prunable data length of 42 kilobytes.

### Properties Files

The behavior of some API calls is affected by property settings loaded from files in the _nxt/conf_ directory during Blue0x server intialization. This directory contains the _nxt-default.properties_ and _logging-default.properties_ files, both of which contain default property settings along with documentation. 

It is recommended not to modify default properties files because they can be overwritten during software updates. Instead, properties in the default files can be overridden by including them in optional _nxt.properties_ and _logging.properties_ files in the same directory. For example, a _nxt.properties_ file can be created with the contents:

nxt.isTestnet=true

This causes the Blue0x server to connect to the Testnet instead of the Mainnet.

### Admin Password

Some API functions take an adminPassword parameter, which must match the nxt.adminPassword property unless the nxt.disableAdminPassword property is set to true or the API server only listens on the localhost interface (when the nxt.apiServerHost property is set to 127.0.0.1).

All [Debug Operations](#debug-operations "The Blue0x API") require adminPassword since they require some kind of privilege. On some functions adminPassword is used so you can override maximum allowed value for lastIndex parameter, which is set to 100 by default by the nxt.maxAPIRecords property. Giving you the option to retrieve more than objects per request.

### Roaming and Light Client Modes

The remote node to use when in roaming and light client modes is selected randomly, but can be changed manually in the UI, or using the [Set API Proxy Peer](#set-api-proxy-peer "The Blue0x API") API, or forced to a specific peer using the _nxt.forceAPIProxyServerURL_ property.

Remote nodes can be blacklisted from the UI, or using the [Blacklist API Proxy Peer](#blacklist-api-proxy-peer "The Blue0x API") API. This blacklisting is independent from peer blacklisting. The API proxy blacklisting period can be set using the _nxt.apiProxyBlacklistingPeriod_ property (default 1800000 milliseconds).

API requests that require sending the secret phrase, shared key, or admin password to the server, for features like forging, shuffling, or running a funding monitor, are disabled when in roaming or light client mode.

Create Transaction
--------------------

The following API calls create Blue0x transactions using HTTP POST requests. Follow the links for examples and HTTP POST request parameters specific to each call. Refer to the sections below for [HTTP POST request parameters](#create-transaction-request "The Blue0x API") and [JSON response fields](index.md#create-transaction-response "The Blue0x API") common to all calls that create transactions. Calls in _italics_ are phasing-safe (refer to [Get Constants](#get-constants "The Blue0x API") and [Create Phasing Poll](#create-phasing-poll "The Blue0x API"))

*   _[Send Money](#send-money "The Blue0x API")_
*   _[Set Account Information](#set-account-information "The Blue0x API")_
*   _[Set Account Property](#set-account-property "The Blue0x API")_
*   [Buy / Sell Alias](#buy-sell-alias "The Blue0x API")
*   [Delete Alias](#delete-alias "The Blue0x API")
*   [Set Alias](#set-alias "The Blue0x API")
*   [Send Message](#send-message "The Blue0x API")
*   _[Cancel Order](#cancel-order "The Blue0x API")_
*   _[Dividend Payment](#dividend-payment "The Blue0x API")_
*   _[Issue Asset](#issue-asset "The Blue0x API")_
*   _[Place Order](#place-order "The Blue0x API")_
*   _[Transfer Asset](#transfer-asset "The Blue0x API")_
*   _[DGS Delisting](#dgs-delisting "The Blue0x API")_
*   [DGS Delivery](#dgs-delivery "The Blue0x API")
*   [DGS Feedback](#dgs-feedback "The Blue0x API")
*   _[DGS Listing](#dgs-listing "The Blue0x API")_
*   [DGS PriceChange](#dgs-pricechange "The Blue0x API")
*   [DGS Purchase](#dgs-purchase "The Blue0x API")
*   [DGS Quantity Change](#dgs-quantity-change "The Blue0x API")
*   [DGS Refund](#dgs-refund "The Blue0x API")
*   _[Lease Balance](#lease-balance "The Blue0x API")_
*   [Currency Buy / Sell](#currency-buy-Sell "The Blue0x API")
*   [Currency Mint](#currency-mint "The Blue0x API")
*   [Currency Reserve Claim](#currency-reserve-claim "The Blue0x API")
*   [Currency Reserve Increase](#currency-reserve-increase "The Blue0x API")
*   [Delete Currency](#delete-currency "The Blue0x API")
*   [Issue Currency](#issue-currency "The Blue0x API")
*   [Publish Exchange Offer](#publish-exchange-offer "The Blue0x API")
*   [Transfer Currency](#transfer-currency "The Blue0x API")
*   [Create Poll](#create-poll "The Blue0x API")
*   [Cast Vote](#cast_vote "The Blue0x API")
*   _[Approve Transaction](#approve-transaction "The Blue0x API")_
*   [Extend Tagged Data](#extend-tagged-data "The Blue0x API")
*   [Upload Tagged Data](#upload-tagged-data "The Blue0x API")

### Create Transaction Request

The following HTTP POST parameters are common to all API calls that create transactions:

*   _secretPhrase_ is the secret passphrase of the account (optional, but transaction neither signed nor broadcast if omitted)
*   _publicKey_ is the public key of the account (optional if _secretPhrase_ provided)
*   _feeNQT_ is the fee (in NQT) for the transaction:
    *   minimum 1000 BLX for [Issue Asset](#issue-asset "The Blue0x API"), unless singleton asset is issued, for which the fee is 1 BLX
    *   2 BLX in base fee for [Set Alias](#set-alias "The Blue0x API"), with 2 BLX additional fee for each 32 chars of name plus URI total length, after the first 32 chars
    *   \[25000, 1000, 40\] BLX for \[3-letter, 4-letter, 5-letter\] [Issue Currency](#issue-currency "The Blue0x API")
    *   40 BLX for re-issue of any currency
    *   10 BLX for a [Create Poll](#create-poll "The Blue0x API"), including 20 options and total size of poll name plus poll description plus total option length not exceeding 320 chars. For each option above 20, an additional fee of 1 BLX, and for each 32 chars after 320, an additional fee of 2 BLX.
    *   \[2, 21\] BLX for a \[basic, required-minimum-balance\] [Create Phasing Poll](#create-phasing-poll "The Blue0x API"). 1 BLX will be added for each option (answer) beyond 20, and 1 BLX for each 32 bytes of hashedSecret or linkedFullHash fields.
    *   1 BLX for the first 32 bytes of a unencrypted non-prunable [message](#send-message "The Blue0x API"), 1 BLX for each additional 32 bytes
    *   2 BLX for the first 32 bytes of an encrypted non-prunable [message](#send-message "The Blue0x API"), 1 BLX for each additional 32 bytes. The length is measured excluding the nonce and the 16 byte AES initialization vector.
    *   1 BLX for the first 1024 bytes of a prunable [message](#send-message "The Blue0x API"), 0.1 BLX for each additional 1024 prunable bytes
    *   1 BLX for [Set Account Info](#set-account-info "The Blue0x API"), including 32 chars, with 2 BLX additional fee for each 32 chars
    *   2 BLX for [DGS Listing](#dgs-listing "The Blue0x API"), including 32 chars of name plust description. With 2 BLX additional fee for each 32 chars.
    *   1 BLX for [DGS Delivery](#dgs-delivery "The Blue0x API"), including 32 bytes of encrypted goods data (AES initialization bytes and nonce excluded). With 2 BLX additional fee for each 32 bytes.
    *   2 BLX for transactions that make use of referencedTransactionFullHash property when creating a new transaction.
    *   1 BLX otherwise, where 1 BLX = 100000000 NQT
*   _deadline_ is the deadline (in minutes) for the transaction to be confirmed, 32767 minutes maximum
*   _referencedTransactionFullHash_ creates a chained transaction, meaning that the current transaction cannot be confirmed unless the referenced transaction is also confirmed (optional)
*   _broadcast_ is set to _false_ to prevent broadcasting the transaction to the network (optional)

**Note:** An optional arbitrary message can be appended to any transaction by adding message-related parameters as in [Send Message](#send-message "The Blue0x API").

**Note:** Any phasing-safe transaction (refer to [Create Transaction](#create-transaction "The Blue0x API")) can have its execution deferred either conditionally or unconditionally (refer to [Create Phasing Poll](#create-phasing-poll "The Blue0x API")).

### Create Transaction Response

The following JSON response fields are common to all API calls that create transactions:

*   _signatureHash_ (S) is a SHA-256 hash of the transaction signature
*   _unsignedTransactionBytes_ (S) are the unsigned transaction bytes
*   _transactionJSON_ (O) is a transaction object (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _broadcasted_ (B) is _true_ if the transaction was broadcast, _false_ otherwise
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _transactionBytes_ (S) are the signed transaction bytes
*   _fullHash_ (S) is the full hash of the signed transaction
*   _transaction_ (S) is the ID of the newly created transaction

Account Operations
--------------------

### Delete Account Property

Deletes an account property. POST only.

**Request:**

*   _requestType_ is _deleteAccountProperty_
*   _property_ is the name of the property
*   _recipient_ is the account where a property should be removed (optional)
*   _setter_ is the account who did set the property (optional)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Account Property](API_Examples.md#delete-account-property "The Blue0x API Examples") example.

### Get Account

Get account information given an account ID.

**Request:**

*   _requestType_ is _getAccount_
*   _account_ is the account ID
*   _includeLessors_ is _true_ to include _lessors_, _lessorsRS_ and _lessorsInfo_ (optional)
*   _includeAssets_ is _true_ to include _assetBalances_ and _unconfirmedAssetBalances_ (optional)
*   _includeCurrencies_ is _true_ to include _accountCurrencies_ (optional)
*   _includeEffectiveBalance_ is _true_ to include _effectiveBalanceNXT_ and _guaranteedBalanceNQT_ (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _unconfirmedBalanceNQT_ (S) is _balanceNQT_ less unconfirmed outgoing transactions, the balance displayed in the client
*   _effectiveBalanceNXT_ (N) is the balance (in BLX) of the account available for forging: the unleased guaranteedBalance of this account plus the leased guaranteedBalance of all lessors to this account
*   _lessorsInfo_ (A) is an array of lessor objects including the fields:
    *   _currentHeightTo_ (S)
    *   _nextHeightFrom_ (S)
    *   _effectiveBalanceNXT_ (S)
    *   _nextLesseeRS_ (S)
    *   _currentLesseeRS_ (S)
    *   _currentHeightFrom_ (S)
    *   _nextHeightTo_ (S)
*   _lessors_ (A) is an array of lessor account IDs
*   _currentLessee_ (S) is the account number of the lessee, if applicable
*   _currentLeasingHeightTo_ (N) is the block height when the lease completes, if applicable
*   _forgedBalanceNQT_ (S) is the balance (in NQT) that the account has forged
*   _balanceNQT_ (S) is the minimally confirmed basic balance (in NQT) of the account
*   _publicKey_ (S) is the public key of the account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _assetBalances_ (A) is an array of asset objects including the fields _balanceQNT_ (S) and _asset_ (S) ID
*   _guaranteedBalanceNQT_ (S) is the balance (in NQT) of the account with at least 1440 confirmations
*   _unconfirmedAssetBalances_ (A) is an array of asset objects including the fields _unconfirmedBalanceQNT_ (S) and _asset_ (S) ID
*   _currentLesseeRS_ (S) is the Reed-Solomon address of the lessee account
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _lessorsRS_ (A) is an array of Reed-Solomon lessor account addresses
*   _accountCurrencies_ (A) is an array of currency objects (refer to [Get Account Currencies](#get-account-currencies "The Blue0x API") for details)
*   _name_ (S) is the name associated with the account, if applicable
*   _description_ (S) is the description of the account, if applicable
*   _account_ (S) is the account number
*   _currentLeasingHeightFrom_ (N) is the block height when the lease starts, if applicable
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)

**Example:** Refer to [Get Account](API_Examples.md#get-account "The Blue0x API Examples") example.

### Get Account Block Count

Get the number of blocks forged by an account.

**Request:**

*   _requestType_ is _getAccountBlockCount_
*   _account_ is an account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfBlocks_ (N) is the number of blocks forged by the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Block Count](API_Examples.md#get-account-block-count "The Blue0x API Examples") example.

### Get Account Block Ids

Get the block IDs of all blocks forged (generated) by an account in reverse block height order.

**Request:**

*   _requestType_ is _getAccountBlockIds_
*   _account_ is the account ID
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first block ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blockIds_ (A) is an array of block IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millsec)

**Example:** Refer to [Get Account Block Ids](API_Examples.md#get-account-block-ids "The Blue0x API Examples") example.

### Get Account Blocks

Get all blocks forged (generated) by an account in reverse block height order.

**Request:**

*   _requestType_ is _getAccountBlocks_
*   _account_ is the account ID
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first block to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block to retrieve (optional)
*   _includeTransactions_ is _true_ to retrieve transaction details, otherwise only transaction IDs are retrieved (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blocks_ (A) is an array of blocks (refer to [Get Block](#get-block "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Blocks](API_Examples.md#get-account-blocks "The Blue0x API Examples") example.

### Get Account Id

Get an account ID given a secret passphrase or public key. POST only.

**Request:**

*   _requestType_ is _getAccountId_
*   _secretPhrase_ is the secret passphrase of the account (optional)
*   _publicKey_ is the public key of the account (optional if _secretPhrase_ provided)

**Response:**

*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _publicKey_ (S) is the public key of the account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _account_ (S) is the account number

**Example:** Refer to [Get Account Id](API_Examples.md#get-account-id "The Blue0x API Examples") example.

### Get Account Ledger

Get multiple account ledger entries.

**Request:**

*   _requestType_ is _getAccountLedger_
*   _account_ is the account ID (optional)
*   _firstIndex_ is a zero-based index to the first block to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block to retrieve (optional)
*   _event_ is the event ID (optional)
*   _eventType_ is a string representing the event type (optional)
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)
*   _includeTransactions_ is true to retrieve transaction details, otherwise only transaction IDs are retrieved (optional)
*   _includeHoldingInfo_ is true to retrieve asset or currency info (optional) with each ledger entry. The default is false.
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _entries_ (A) is an array of ledger objects including the fields:
    *   _change_ (S) is the change in the balance for the holding identified by 'holdingType'
    *   _eventType_ (S) is the event type causing the account change
    *   _ledgerId_ (S) is the ledger entry ID
    *   _holding_ (S) is the item identifier for an asset or currency balance
    *   _isTransactionEvent_ (B) is true if the event is associated with a transaction and false if it is associated with a block.
    *   _balance_ (S) is the balance for the holding identified by 'holdingType'
    *   _holdingType_ (S) is the item being changed (account balance, asset balance or currency balance)
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _block_ (S) is the block ID that created the ledger entry
    *   _event_ (S) is the block or transaction associated with the event
    *   _account_ (S) is the account number
    *   _height_ (N) is the the block height associated with the event
    *   _timestamp_ (N) is the the block timestamp associated with the event
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Ledger](API_Examples.md#get-account-ledger "The Blue0x API Examples") example.

### Get Account Ledger Entry

Get a specific account ledger entry.

**Request:**

*   _requestType_ is _getAccountLedgerEntry_
*   _ledgerId_ is the ledger ID
*   _includeTransactions_ is true to retrieve transaction details, otherwise only transaction IDs are retrieved (optional)
*   _includeHoldingInfo_ is _true_ to retrieve asset or currency info (optional) with the ledger entry. The default is false.

**Response:**

*   _change_ (S) is the change in the balance for the holding identified by 'holdingType'
*   _eventType_ (S) is the event type causing the account change
*   _ledgerId_ (S) is the ledger entry ID
*   _holding_ (S) is the item identifier for an asset or currency balance
*   _isTransactionEvent_ (B) is true if the event is associated with a transaction and false if it is associated with a block.
*   _balance_ (S) is the balance for the holding identified by 'holdingType'
*   _holdingType_ (S) is the item being changed (account balance, asset balance or currency balance)
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _block_ (S) is the block ID that created the ledger entry
*   _event_ (S) is the block or transaction associated with the event
*   _account_ (S) is the account number
*   _height_ (N) is the the block height associated with the event
*   _timestamp_ (N) is the the block timestamp associated with the event
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Ledger Entry](API_Examples.md#get-account-ledger-entry "The Blue0x API Examples") example.

### Get Account Lessors

Get the lessors to an account.

**Request:**

*   _requestType_ is _getAccountLessors_
*   _account_ is the account ID
*   _height_ is the height of the blockchain to determine the lessors (optional, default is last block)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the _height_ must be within 1440 blocks of the last block.

**Response:**

*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _account_ (S) is the account number
*   _height_ (N) is the height of the blockchain
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _lessors_ (A) is an array of lessor objects including the fields:
    *   _lessorRS_ (S)
    *   _lessor_ (S)
    *   _guaranteedBalanceNQT_ (S)

**Example:** Refer to [Get Account Lessors](API_Examples.md#get-account_lessors "The Blue0x API Examples") example.

### Get Account Properties

Get the Account properties for a specific account or setter.

**Request:**

*   _requestType_ is _getAccountProperties_
*   _recipient_ is the account ID to which the property is attached to
*   _setter_ is the account ID who set the property (optional if _recipient_ provided)
*   _property_ is the property key (optional)
*   _firstIndex_ is a zero-based index to the first block to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _setterRS_: (S) is the Reed-Solomon address of the setter account (only if setter is defined in the request)
*   _recipientRS_: (S) is the Reed-Solomon address of the recipient account (only if recipient is defined in the request)
*   _recipient_: (S) is the account number of the recipient account (only if recipient is defined in the request)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _setter_: (S) is the account number of the setter account (only if setter is defined in the request)
*   _properties_: (A) is an array of properties including the fields:
    *   _setterRS_: (S) is the Reed-Solomon address of the setter account (only if setter is omitted in the request)
    *   _recipientRS_: (S) is the Reed-Solomon address of the recipient account (only if recipient is omitted in the request)
    *   _recipient_: (S) is the account number of the recipient account (only if recipient is omitted in the request)
    *   _property_: (S) property name
    *   _setter_: (S) is the account number of the setter account (only if setter is omitted in the request)
    *   _value_: (S) property value

**Example:** Refer to [Get Account Properties](API_Examples.md#get-account-properties "The Blue0x API Examples") example.

### Get Account Public Key

Get the public key associated with an account ID.

**Request:**

*   _requestType_ is _getAccountPublicKey_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _publicKey_ (S) is the 32-byte public key associated with the account, returned as a hex string
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Public Key](API_Examples.md#get-account-public-key "The Blue0x API Examples") example.

### Get Balance

Get the balance of an account.

**Request:**

*   _requestType_ is _getBalance_
*   _account_ is an account ID
*   _includeEffectiveBalance_ is _true_ to include _effectiveBalanceNXT_ and _guaranteedBalanceNQT_ (optional)
*   _height_ is the height to retrieve account balance for, if still available (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _unconfirmedBalanceNQT_ (S) is _balanceNQT_ less unconfirmed outgoing transactions, the balance displayed in the client
*   _guaranteedBalanceNQT_ (S) is the balance (in NQT) of the account with at least 1440 confirmations
*   _effectiveBalanceNXT_ (N) is the balance (in Blue0x) of the account available for forging: the unleased guaranteedBalance of this account plus the leased guaranteedBalance of all lessors to this account
*   _forgedBalanceNQT_ (S) is the balance (in NQT) that the account has forged
*   _balanceNQT_ (S) is the minimally confirmed basic balance (in NQT) of the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Balance](API_Examples.md#get-balance "The Blue0x API Examples") example.

### Get Blockchain Transactions

Get the transactions associated with an account in reverse block timestamp order.

  
**Request:**

*   _requestType_ is _getBlockchainTransactions_
*   _account_ is the account ID
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _type_ is the type of transactions to retrieve (optional)
*   _subtype_ is the subtype of transactions to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last transaction to retrieve (optional)
*   _numberOfConfirmations_ is the required number of confirmations per transaction (optional)
*   _withMessage_ is _true_ to retrieve only only transactions having a message attachment, either non-encrypted or decryptable by the account (optional)
*   _phasedOnly_ is _true_ to retrieve only phased transactions (optional unless _nonPhasedOnly_ provided)
*   _nonPhasedOnly_ is _true_ to retrieve only nonphased transactions (optional unless _phasedOnly_ provided)
*   _includeExpiredPrunable_ is _true' to retrieve pruned data if available (optional)_
*   _includePhasingResult_ is _true_ to retrieve execution status of each phased transaction (optional)
*   _executedOnly_ is _true_ to exclude phased transactions that are not yet executed (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** Refer to [Get Constants](#get-constants "The Blue0x API") for definitions of types and subtypes

**Response:**

*   _transactions_ (A) is an array of transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Blockchain Transactions](API_Examples.md#get-blockchain-transactions "The Blue0x API Examples") example.

### Get Guaranteed Balance

Get the balance of an account that is confirmed at least a specified number of times.

**Request:**

*   _requestType_ is _getGuaranteedBalance_
*   _account_ is an account ID
*   _numberOfConfirmations_ is the minimum number of confirmations for a transaction to be included in the guaranteed balance (optional, if omitted or zero then minimally confirmed transactions are included)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _guaranteedBalanceNQT_ (S) is the balance (in NQT) of the account with at least _numberOfConfirmations_ confirmations
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Guaranteed Balance](API_Examples.md#get-guaranteed-balance "The Blue0x API Examples") example.

  

### Get Unconfirmed Transaction Ids

Get a list of unconfirmed transaction IDs associated with an account.

**Request:**

*   _requestType_ is _getUnconfirmedTransactionIds_
*   _account_ is one account ID (optional)
*   _account_ is one account ID (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)
*   _firstIndex_ is a zero-based index to the first transaction ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last transaction ID to retrieve (optional)

**Response:**

*   _unconfirmedTransactionIds_ (A) is an array of unconfirmed transaction IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Unconfirmed Transaction Ids](API_Examples.md#get-unconfirmed-transaction-ids "The Blue0x API Examples") example.

### Get Unconfirmed Transactions

Get a list of unconfirmed transactions associated with an account.

**Request:**

*   _requestType_ is _getUnconfirmedTransactions_
*   _account_ is one account ID (optional)
*   _account_ is one account ID (optional)

⋮

*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)
*   _firstIndex_ is a zero-based index to the first unconfirmed transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last unconfirmed transaction to retrieve (optional)

**Response:**

*   _unconfirmedTransactions_ (A) is an array of unconfirmed transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Unconfirmed Transactions](API_Examples.md#get-unconfirmed-transactions "The Blue0x API Examples") example.

### Search Accounts

Get accounts having a name or description that match a given query in reverse relevance order.

**Request:**

*   _requestType_ is _searchAccounts_
*   _query_ is a full text query on the account fields _name_ (S) and _description_ (S) in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _firstIndex_ is a zero-based index to the first account to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last account to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _accounts_ (A) is an array of account objects with the following fields:
    *   _account_ (S) is the account number
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _name_ (S) is the name of the account
    *   _description_ (S) is the description of the account (if applicable)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Accounts](API_Examples.md#search-accounts "The Blue0x API Examples") example.

### Send Money

Send Blue0x to another account. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _sendMoney_
*   _amountNQT_ is the amount (in NQT) in the transaction
*   _recipient_ is the account ID of the recipient
*   _recipientPublicKey_ is the public key of the receiving account (optional, enhances security of a new account)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Send Money](API_Examples.md#send-money "The Blue0x API Examples") example.

#### Send BLX

Refer to [Send Money](#send-money "The Blue0x API").

### Set Account Info

Set account information. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setAccountInfo_
*   _name_ is a name to associate with the account (optional)
*   _description_ is a description to associate with the account (optional)
*   _messagePatternRegex_ is a regular expression pattern following the java.util.regex.Pattern specification; incoming transactions are only accepted if they contain a plain text message which matches this pattern (disabled indefinitely due to a security issue)
*   _messagePatternFlags_ is a bitmask of java.util.regex.Pattern flags, such as 2 (Pattern.CASE\-iNSENSITIVE)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Set Account Info](API_Examples.md#set-account-info "The Blue0x API Examples") example.

### Set Account Property

Set account property. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setAccountProperty_
*   _recipient_ is the account ID of the recipient.
*   _property_ is the property name.
*   _value_ is the property value.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Set Account Property](API_Examples.md#set-account-property "The Blue0x API Examples") example.

### Start Funding Monitor

Starts a funding monitor that will transfer BLX, an ASSET or a CURRENCY from the funding account to a recipient account when the amount held by the recipient account drops below the threshold. The transfer will not be done until the current block height is greater than equal to the block height of the last transfer plus the interval. The funding account is identified by the secret phrase.

The recipient accounts are identified by the specified account property (see [Set Account Property](#set-account-property "The Blue0x API")). Each account that has this property set by the funding account will be monitored for changes. The property value can be omitted or it can consist of a JSON string containing one or more values in the format: {"amount":long,"threshold":long,"interval":integer} . POST only.

**Request:**

*   _requestType_ is _startFundingMonitor_
*   _property_ is the name of the account property
*   _amount_ is the amount to fund the recipient account with (in NQT or QNT)
*   _threshold_ is the threshold
*   _interval_ is the the number of blocks to wait after before funding the recipient
*   _secretPhrase_ is the secret phrase of the funding account
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)

**Response:**

*   _started_ (B) is _true_ if the monitor has been started
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Start Funding Monitor](API_Examples.md#start-funding-monitor "The Blue0x API Examples") example.

### Stop Funding Monitor

Stop a funding monitor. When the secret phrase is specified, a single monitor will be stopped. The monitor is identified by the secret phrase, holding and account property. The administrator password is not required and will be ignored.

When the administrator password is specified, a single monitor can be stopped by specifying the funding account, holding and account property. If no account is specified, all monitors will be stopped.

The holding type and account property name must be specified when the secret phrase or account is specified. Holding type codes are listed in getConstants. In addition, the holding identifier must be specified when the holding type is ASSET or CURRENCY. POST only.

**Request:**

*   _requestType_ is _stopFundingMonitor_
*   _secretPhrase_ is the secret phrase of the funding account, used to stop a single monitor. (optional)
*   _adminPassword_ is the admin password, used to stop a single monitor or all monitors (optional if _secretPhrase_ is provided)
*   _property_ is the name of the account property (optional)
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)
*   _account_ is the account ID (optional)

**Response:**

*   _stopped_ (N) is the number of the monitors that have been stopped
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Stop Funding Monitor](API_Examples.md#stop-funding-monitor "The Blue0x API Examples") example.

Account Control Operations
----------------------------

### Get All Phasing Only Controls

Retrieve all accounts subject to phasing control with their respective restrictions.

**Request:**

*   _requestType_ is _getAllPhasingOnlyControls_
*   _firstIndex_ is a zero-based index to the first block ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _phasingOnlyControls_ (A) is an array with phasing only controls objects (Refer to [Get Phasing Only Control](#get-phasing-only-control "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Phasing Only Controls](API_Examples.md#get-all-phasing-only-controls "The Blue0x API Examples") example.

### Get Phasing Only Control

Retrieve phasing control with their respective restrictions for a specific account.

**Request:**

*   _requestType_ is _getPhasingOnlyControl_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _account_ (S) is the account number
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _quorum_ (S) is the minimum number of votes needed to approve the transaction
*   _whitelist_ (A) is an array with the whitelisted accounts including the fields:
    *   _whitelisted_ (S) is the account number
    *   _whitelistedRS_ (S) is the Reed-Solomon address of the account
*   _maxFees_ (S) is the maximum fees the account can spend per block
*   _minDuration_ (N) is the minimum duration of the phasing period
*   _maxDuration_ (N) is the maximum duration of the phasing period
*   _votingModel_ (N) is an integer code for the method of approval
*   _minBalance_ (S) is the minimum balance (in NQT or QNT) required for voting
*   _minBalanceModel_ (N) is the minimum balance model
*   _holding_ (S) is the asset or currency ID (only included if holding != 0)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Phasing Only Control](API_Examples.md#get-phasing-only-control "The Blue0x API Examples") example.

### Set Phasing Only Control

Sets (or removes) phasing control for a specific account. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setPhasingOnlyControl_
*   _controlVotingModel_ is the voting model or -1 to remove phasing control
*   _controlQuorum_ is the expected quorum (optional)
*   _controlMinBalance_ is the expected minimum balance (optional)
*   _controlMinBalanceModel_ is the expected minimum balance model (optional)
*   _controlHolding_ is the holding ID (optional)
*   _controlWhitelisted_ is the whitelisted accounts (optional, multiple values)
*   _controlWhitelisted_ is the whitelisted accounts (optional, multiple values)
*   _controlMaxFees_ is the maximum allowed accumulated total fees for not yet finished phased transactions (optional)
*   _controlMinDuration_ is the minimum duration in block height (optional)
*   _controlMaxDuration_ is the maximum phasing duration in block height (optional)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Set Phasing Only Control](API_Examples.md#set-phasing-only-control "The Blue0x API Examples") example.

Alias Operations
------------------

### Buy / Sell Alias

Buy or sell an alias. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is either _buyAlias_ or _sellAlias_
*   _alias_ is the ID of the alias (optional)
*   _aliasName_ is the alias name (optional if _alias_ provided)
*   _priceNQT_ is the asking price (in NQT) of the alias (_sellAlias_ only)
*   _amountNQT_ is the amount (in NQT) offered for an alias for sale (_buyAlias_ only)
*   _recipient_ is the account ID of the recipient (only required for _sellAlias_ and only if there is a designated recipient)
*   _recipientPublicKey_ is the public key of the recipient account (only applicable if _recipient_ provided; optional, enhances security of a new account)

**Note**: An alias can be transferred rather than sold by setting _priceNQT_ to zero. A pending sale can be canceled by selling again to self for a price of zero.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Buy / Sell Alias](API_Examples.md#buy-sell-alias "The Blue0x API Examples") example.

#### Buy Alias

Refer to [Buy / Sell Alias](#buy-sell-alias "The Blue0x API").

#### Sell Alias

Refer to [Buy / Sell Alias](#buy-sell-alias "The Blue0x API").

### Set Alias

Create and/or assign an alias. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _setAlias_
*   _aliasName_ is the alias name
*   _aliasURI_ is the alias URI (e.g. [https://www.google.com/](https://www.google.com/))

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the alias ID.

**Example:** Refer to [Set Alias](API_Examples.md#set-alias "The Blue0x API Examples") example.

### Delete Alias

Delete an alias given an alias ID or name. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _deleteAlias_
*   _alias_ is the alias ID (optional)
*   _aliasName_ is the alias name to be deleted (optional if _alias_ provided)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Alias](API_Examples.md#delete-alias "The Blue0x API Examples") example.

### Get Alias

Get information about a given alias

**Request:**

*   _requestType_ is _getAlias_
*   _alias_ is the alias ID (optional)
*   _aliasName_ is the name of the alias (optional if _alias_ provided)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _timestamp_ (N) is the time (in seconds since the genesis block) when the alias was created or last transferred
*   _aliasName_ (S) is the name of the alias
*   _account_ (S) is the number of the account that owns the alias
*   _accountRS_ (S) is the Reed-Solomon address of the account that owns the alias
*   _aliasURI_ (S) is what the alias points to, in URI format
*   _alias_ (S) is the alias ID
*   _priceNQT_ (S) is the asking price (in NQT) of the alias if it is for sale
*   _buyer_ (S) is the account number of the buyer if the alias is for sale and a buyer is specified
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Alias](API_Examples.md#get-alias "The Blue0x API Examples") example.

### Get Alias Count

Get the number of aliases owned by an account given the account ID.

**Request:**

*   _requestType_ is _getAliasCount_
*   _account_ is the account ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfAliases_ (N) is the number of aliases owned by the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Alias Count](API_Examples.md#get-alias-count "The Blue0x API Examples") example.

### Get Aliases

Get information on aliases owned by a given account in alias name order.

**Request:**

*   _requestType_ is _getAliases_
*   _account_ is the ID of the account that owns the aliases
*   _timestamp_ is the earliest creation time (in seconds since the genesis block) of the aliases (optional)
*   _firstIndex_ is a zero-based index to the first alias to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last alias to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _aliases_ (A) is an array of alias objects (refer to [Get Alias](#get-alias "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Aliases](API_Examples.md#get-aliases "The Blue0x API Examples") example.

### Get Aliases Like

Get all aliases starting with a given prefix in alias name order.

**Request:**

*   _requestType_ is _getAliasesLike_
*   _aliasPrefix_ is the prefix (at least 2 characters long) of the _aliasName_
*   _firstIndex_ is a zero-based index to the first alias to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last alias to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _aliases_ (A) is an array of alias objects (refer to [Get Alias](#get-alias "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Aliases Like](API_Examples.md#get-aliases-like "The Blue0x API Examples") example.

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

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

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

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

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

Asset Exchange Operations
---------------------------

### Cancel Order

Cancel an existing asset order. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is either _cancelBidOrder_ or _cancelAskOrder_
*   _order_ is the order ID of the order being canceled

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Cancel Order](API_Examples.md#cancel-order "The Blue0x API Examples") example.

#### Cancel Ask Order

Refer to [Cancel Order](#cancel-order "The Blue0x API").

#### Cancel Bid Order

Refer to [Cancel Order](#cancel-order "The Blue0x API").

### Delete Asset Shares

Permanently deletes a specified quantity of owned asset shares.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _deleteAssetShares_
*   _asset_ is the asset ID
*   _quantityQNT_ is the quantity (in QNT) of the asset to be deleted

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Asset Shares](API_Examples.md#delete-asset-shares "The Blue0x API Examples") example.

### Dividend Payment

Pay dividend to all shareholders of an asset. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dividendPayment_
*   _asset_ is the asset ID
*   _height_ is the blockchain height at which asset holders shares will be counted (must be less than 1440 blocks in the past)
*   _amountNQTPerQNT_ is dividend amount (in NQT per QNT of the asset)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Dividend Payment](API_Examples.md#dividend-payment "The Blue0x API Examples") example.

### Get Account Asset Count

Get the number of assets owned by an account given the account ID.

**Request:**

*   _requestType_ is _getAccountAssetCount_
*   _account_ is the account ID
*   _height_ is the height of the blockchain to determine the asset count (optional, default is last block)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _numberOfAssets_ (N) is the number of assets owned by the account
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Asset Count](API_Examples.md#get-account-asset-count "The Blue0x API Examples") example.

### Get Account Assets

Get the assets owned by a given account in reverse quantity order.

**Request:**

*   _requestType_ is _getAccountAssets_
*   _account_ is the account ID
*   _asset_ is an asset ID filter (optional)
*   _height_ is the blockchain height at which to retrieve balances (optional, default is the last block in the blockchain)
*   _includeAssetInfo_ is _true_ if asset information is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _accountAssets_ (A) is an array of asset objects (unless the _asset_ parameter is specified) with the following fields for each asset:
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset
    *   _unconfirmedQuantityQNT_ (S) is the unconfirmed quantity (in QNT) of the asset
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _name_ (S) is the asset name (if _includeAssetInfo_ is _true_)
    *   _asset_ (S) is the asset ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Assets](API_Examples.md#get-account-assets "The Blue0x API Examples") example.

### Get Account Current Order Ids

Get current asset order IDs given an account ID in reverse block height order.

**Request:**

*   _requestType_ is either _getAccountCurrentBidOrderIds_ or _getAccountCurrentAskOrderIds_
*   _account_ is the account ID
*   _asset_ is an asset ID filter (optional)
*   _firstIndex_ is a zero-based index to the first order ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrderIds_ or _askOrderIds_ (A) is an array of order IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Current Order Ids](API_Examples.md#get-account-current-order-ids "The Blue0x API Examples") example.

#### Get Account Current Ask Order Ids

Refer to [Get Account Current Order Ids](#get-account-current-order-ids "The Blue0x API").

#### Get Account Current Bid Order Ids

Refer to [Get Account Current Order Ids](#get-account-current-order-ids "The Blue0x API").

### Get Account Current Orders

Get current asset orders given an account ID in reverse block height order.

**Request:**

*   _requestType_ is either _getAccountCurrentBidOrders_ or _getAccountCurrentAskOrders_
*   _account_ is the account ID
*   _asset_ is an asset ID filter (optional)
*   _firstIndex_ is a zero-based index to the first order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrders_ or _askOrders_ (A) is an array of order objects (refer to [Get Order](#get-order "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Current Orders](API_Examples.md#get-account-current-orders "The Blue0x API Examples") example.

#### Get Account Current Ask Orders

Refer to [Get Account Current Orders](#get-account-current-orders "The Blue0x API").

#### Get Account Current Bid Orders

Refer to [Get Account Current Orders](#get-account-current-orders "The Blue0x API").

### Get All Assets

Get all assets in the exchange in reverse block height of creation order.

**Request:**

*   _requestType_ is _getAllAssets_
*   _firstIndex_ is a zero-based index to the first asset to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Assets](API_Examples.md#get-all-assets "The Blue0x API Examples") example.

### Get All Open Orders

Get all open bid/ask orders in reverse block height order.

**Request:**

*   _requestType_ is either _getAllOpenBidOrders_ or _getAllOpenAskOrders_
*   _firstIndex_ is a zero-based index to the first order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _openOrders_ (A) is an array of order objects (refer to [Get Order](#get-order "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Open Orders](API_Examples.md#get-all-open-orders "The Blue0x API Examples") example.

#### Get All Open Ask Orders

Refer to [Get All Open Orders](#get-all-open-orders "The Blue0x API").

#### Get All Open Bid Orders

Refer to [Get All Open Orders](#get-all-open-orders "The Blue0x API").

### Get All Trades

Get all trades since a given timestamp in reverse block height order.

**Request:**

*   _requestType_ is _getAllTrades_
*   _timestamp_ is the timestamp (in seconds since the genesis block) to begin retrieving trades (optional, default 0)
*   _firstIndex_ is a zero-based index to the first trade to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last trade to retrieve (optional)
*   _includeAssetInfo_ is _true_ if asset information is to be included in the result (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If _timestamp_ is omitted or zero, and no index is given, all trades in the entire blockchain will be retrieved, which may timeout or crash your system.

**Response:**

*   _trades_ (A) is an array of trade objects (refer to [Get Trades](#get-trades "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Trades](API_Examples.md#get-all-trades "The Blue0x API Examples") example.

### Get Asset

Get asset information given an asset ID.

**Request:**

*   _requestType_ is _getAsset_
*   _asset_ is the asset ID
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _account_ (S) is the number of the account that issued the asset
*   _accountRS_ (S) is the Reed-Solomon address of the account that issued the asset
*   _name_ (S) is the asset name
*   _description_ (S) is the asset description
*   _quantityQNT_ (S) is the total asset quantity (in QNT) in existence
*   _asset_ (N) is the asset ID
*   _decimals_ (N) is the number of decimal places used by the asset
*   _numberOfAccounts_ (N) is the number of accounts that own the asset
*   _numberOfTrades_ (N) is the number of trades of this asset
*   _numberOfTransfers_ (N) is the number of transfers of this asset
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset](API_Examples.md#get-asset "The Blue0x API Examples") example.

### Get Asset Account Count

Get the number of accounts that own an asset given the asset ID.

**Request:**

*   _requestType_ is _getAssetAccountCount_
*   _asset_ is the asset ID
*   _height_ is the height of the blockchain to determine the account count (optional, default is last block)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _numberOfAccounts_ (N) is the number of accounts that own the asset
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Account Count](API_Examples.md#get-asset-account-count "The Blue0x API Examples") example.

### Get Asset Accounts

Get the accounts that own an asset given the asset ID in reverse quantity order.

**Request:**

*   _requestType_ is _getAssetAccounts_
*   _asset_ is the asset ID
*   _height_ is the height of the blockchain to determine the accounts (optional, default is last block)
*   _firstIndex_ is a zero-based index to the first account to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last account to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If table trimming is enabled (default), the height must be within 1440 blocks of the last block.

**Response:**

*   _accountAssets_ (A) is an array of asset objects with the following fields for each asset:
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset
    *   _accountRS_ (S) is the Reed-Solomon address of the account that owns the asset
    *   _unconfirmedQuantityQNT_ (S) is the unconfirmed quantity (in QNT) of the asset
    *   _asset_ (S) is the asset ID
    *   _account_ (S) is the number of the account that owns the asset
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Accounts](API_Examples.md#get-asset-accounts "The Blue0x API Examples") example.

### Get Asset Deletes

Get asset deletions for a specific asset or account.

**Request:**

*   _requestType_ is _getAssetDeletes_
*   _asset_ is the asset ID (optional if account is provided)
*   _account_ is the account ID (optional if asset is provided)
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _timestamp_ is the earliest deletion (in seconds since the genesis block) to retrieve (optional)
*   _includeAssetInfo_ is _true_ if asset information is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _deletes_ (A) is an array of asset delete objects with following properties:
    *   _quantityQNT_ (S) is the number of shares that was deleted
    *   _assetDelete_ (S) is the transaction ID
    *   _account_ (S) is the account ID
    *   _accountRS_ (S) is the account Reed Solomon address
    *   _asset_ (S) is the asset ID
    *   _height_ (N) is the block height of the delete
    *   _timestamp_ (N) is the block timestamp of the delete
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _name_ (S) is the asset name (if _includeAssetInfo_ is _true_)

**Example:** Refer to [Get Asset Deletes](API_Examples.md#get-asset-deletes "The Blue0x API Examples") example.

### Get Asset Dividends

Get the dividend payment history for a specific asset.

**Request:**

*   _requestType_ is _getAssetDividends_
*   _asset_ is the asset ID
*   _firstIndex_ is a zero-based index to the first dividend payment to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last dividend payment to retrieve (optional)
*   _timestamp_ is the earliest dividend payment (in seconds since the genesis block) to retrieve (optional)
*   _adminPassword_ is a string with the admin password (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _dividends_ (A) is an array of dividend transactions with the following properties:
    *   _assetDividend_ (S) is the dividend payment transaction ID
    *   _numberOfAccounts_ (N) is the number of accounts that received a dividend
    *   _amountNQTPerQNT_ (S) is the amount of BLX (in NQT) paid per quantity (in QNT) of the asset
    *   _totalDividend_ (S) is the total amount of BLX (in NQT) sent in the dividend payment
    *   _dividendHeight_ (N) is the block height of the dividend calculation
    *   _asset_ (S) is the asset ID
    *   _height_ (N) is the block height of the dividend payment
    *   _timestamp_ (N) is the block timestamp of the dividend payment

**Example:** Refer to [Get Asset Dividends](API_Examples.md#get-asset-dividends "The Blue0x API Examples") example.

### Get Asset Ids

Get the IDs of all assets in the exchange in reverse block height of creation order.

**Request:**

*   _requestType_ is _getAssetIds_
*   _firstIndex_ is a zero-based index to the first asset ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Ids](API_Examples.md#get-asset-ids "The Blue0x API Examples") example.

### Get Asset Transfers

Get transfers associated with a given asset and/or account in reverse block height order (or in the expected order of execution for expected transfers).

**Request:**

*   _requestType_ is either _getAssetTransfers_ or _getExpectedAssetTransfers_, where expected transfers are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _asset_ is the asset ID (optional)
*   _account_ is the account ID (optional if _asset_ provided)
*   _timestamp_ is the earliest transfer (in seconds since the genesis block) to retrieve (optional, does not apply to expected transfers)
*   _firstIndex_ is a zero-based index to the first transfer to retrieve (optional, does not apply to expected transfers)
*   _lastIndex_ is a zero-based index to the last transfer to retrieve (optional, does not apply to expected transfers)
*   _includeAssetInfo_ is _true_ if the _decimals_ and _name_ fields are to be included (optional, does not apply to expected transfers)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transfers_ (A) is an array of transfer objects with the following fields for each transfer:
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset traded
    *   _senderRS_ (S) is the Reed-Solomon address of the sender
    *   _assetTransfer_ (S) is the transaction ID of the asset transfer
    *   _sender_ (S) is the account number of the sender
    *   _recipientRS_ (S) is the Reed-Solomon address of the recipient
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _recipient_ (S) is the account number of the recipient
    *   _name_ (S) is the name of the asset (if _includeAssetInfo_ is _true_)
    *   _asset_ (S) is the asset ID
    *   _height_ (N) is the height of the transfer block
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the transfer block, does not apply to an expected transfer
    *   _phased_ (B) is _true_ if the transaction is phased, _false_ otherwise, applies only to an expected transfer
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Asset Transfers](API_Examples.md#get-asset-transfers "The Blue0x API Examples") example.

#### Get Expected Asset Transfers

Refer to [Get Asset Transfers](#get-asset-transfers "The Blue0x API").

### Get Assets

Get asset information given multiple asset IDs

**Request:**

*   _requestType_ is _getAssets_
*   _assets_ is one the multiple asset IDs
*   _assets_ is one the multiple asset IDs
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Assets](API_Examples.md#get-assets "The Blue0x API Examples") example.

### Get Assets By Issuer

Get asset information given multiple creation account IDs in reverse block height of creation order.

**Request:**

*   _requestType_ is _getAssetsByIssuer_
*   _account_ is one of the multiple account IDs
*   _account_ is one of the multiple account IDs
*   _firstIndex_ is a zero-based index to the first asset to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Assets By Issuer](API_Examples.md#get-assets-by-issuer "The Blue0x API Examples") example.

### Get Expected Asset Deletes

Gets asset deletes which are expected to be executed in the next block.

**Request:**

*   _requestType_ is either _getExpectedAssetDeletes_
*   _asset_ is the asset ID (optional)
*   _account_ is the account ID (optional)
*   _firstIndex_ is a zero-based index to the first phased transaction to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last phased transaction to retrieve (optional)
*   _timestamp_ is the earliest deletion (in seconds since the genesis block) to retrieve (optional)
*   _includeAssetInfo_ is _true_ if asset information is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _deletes_ (A) is an array of expected asset delete objects with following properties:
    *   _assetDelete_ (S) is the transaction ID
    *   _asset_ (S) is the asset ID
    *   _account_ (S) is the account ID
    *   _accountRS_ (S) is the account Reed Solomon address
    *   _quantityQNT_ (S) is the number of shares that will be deleted
    *   _decimals_ (N) is the number of decimal places used by the asset (if _includeAssetInfo_ is _true_)
    *   _name_ (S) is the asset name (if _includeAssetInfo_ is _true_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Expected Asset Deletes](API_Examples.md#get-expected-asset-deletes "The Blue0x API Examples") example.

### Get Order

Get a bid/ask order given an order ID.

**Request:**

*   _requestType_ is either _getBidOrder_ or _getAskOrder_
*   _order_ is the Order ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _account_ (S) is the account number associated with the order
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _asset_ (S) is the ID of the asset being ordered
*   _quantityQNT_ (S) is the order quantity (in QNT)
*   _priceNQT_ (S) is the order price (in NQT)
*   _height_ (N) is the block height of the order transaction
*   _transactionHeight_ (N) is the transaction height
*   _transactionIndex_ (N) is a zero-based index giving the order of the transaction in its block
*   _order_ (S) is the ID of the order
*   _type_ (S) is the type of order (_bid_ or _ask_)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Order](API_Examples.md#get-order "The Blue0x API Examples") example.

#### Get Ask Order

Refer to [Get Order](#get-order "The Blue0x API").

#### Get Bid Order

Refer to [Get Order](#get-order "The Blue0x API").

### Get Order Ids

Get bid/ask order IDs given an asset ID, in order of decreasing bid price or increasing ask price.

**Request:**

*   _requestType_ is either _getBidOrderIds_ or _getAskOrderIds_
*   _asset_ is the asset ID
*   _firstIndex_ is a zero-based index to the first order ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last order ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrderIds_ or _askOrderIds_ (A) is an array of order IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Order Ids](API_Examples.md#get-order-ids "The Blue0x API Examples") example.

#### Get Ask Order Ids

Refer to [Get Order Ids](#get-order-ids "The Blue0x API").

#### Get Bid Order Ids

Refer to [Get Order Ids](#get-order-ids "The Blue0x API").

### Get Orders

Get bid/ask orders given an asset ID, in order of decreasing bid price or increasing ask price (if _sortByPrice_ is _true_ for expected orders, otherwise in the expected order of execution).

**Request:**

*   _requestType_ is one of _getBidOrders_, _getAskOrders_, _getExpectedBidOrders_ or _getExpectedAskOrders_, where expected orders are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _asset_ is the asset ID
*   _sortByPrice_ is _true_ to sort by price (optional, applies only to expected orders, which are returned in expected order of execution by default)
*   _showExpectedCancellations_ is _true_ to include orders that are expected to be cancelled in the next block, based on the content of the unconfirmed transactions pool and the phased transactions expected to finish in the next block (optional, does not apply to expected orders)
*   _firstIndex_ is a zero-based index to the first order to retrieve (optional, does not apply to expected orders)
*   _lastIndex_ is a zero-based index to the last order to retrieve (optional, does not apply to expected orders)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _bidOrders_ or _askOrders_ (A) is an array of order objects (refer to [Get Order](#get-order "The Blue0x API") for details) with the following additional field only for an expected order:
    *   _phased_ (B) is _true_ if the order is phased, _false_ otherwise
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Orders](API_Examples.md#get-orders "The Blue0x API Examples") example.

#### Get Ask Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

#### Get Bid Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

#### Get Expected Ask Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

#### Get Expected Bid Orders

Refer to [Get Orders](#get-orders "The Blue0x API").

### Get Expected Order Cancellations

Get all expected order cancellations in the order in which they are expected to be executed.

**Request:**

*   _requestType_ is _getExpectedOrderCancellations_, where expected cancellations are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _orderCancellations_ (A) is an array of order cancellation objects with the following fields for each transfer:
    *   _account_ (S) is the cancelling account number
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _order_ (S) is the ID of the order to be cancelled
    *   _height_ (N) is the block height of the order cancellation transaction
    *   _phased_ (B) is _true_ if the order cancellation transaction is phased
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Expected Order Cancellations](API_Examples.md#get-expected-order-cancellations "The Blue0x API Examples") example.

### Get Last Trades

Get the last trade of each of multiple assets.

**Request:**

*   _requestType_ is _getLastTrades_
*   _assets_ is one of multiple asset IDs
*   _assets_ is one of multiple asset IDs
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _trades_ (A) is an array of trade objects (refer to [Get Trades](#get-trades "The Blue0x API") without _name_ and _decimals_ for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Last Trades](API_Examples.md#get_last-trades "The Blue0x API Examples") example.

### Get Order Trades

Get all trades that were executed as a result of a given _askOrder_ and/or _bidOrder_ in reverse block height order.

**Request:**

*   _requestType_ is _getOrderTrades_
*   _askOrder_ is an ask order ID (optional)
*   _bidOrder_ is a bid order ID (optional if _askOrder_ provided)
*   _firstIndex_ is a zero-based index to the first trade to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last trade to retrieve (optional)
*   _includeAssetInfo_ is _true_ if the _decimals_ and _name_ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Trades](#get-trades "The Blue0x API").

**Example:** Refer to [Get Order Trades](API_Examples.md#get-order-trades "The Blue0x API Examples") example.

### Get Trades

Get trades associated with a given asset and/or account in reverse block height order.

**Request:**

*   _requestType_ is _getTrades_
*   _asset_ is the asset ID (optional)
*   _account_ is the account ID (optional if _asset_ provided)
*   _firstIndex_ is a zero-based index to the first trade to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last trade to retrieve (optional)
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _includeAssetInfo_ is _true_ if the _decimals_ and _name_ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _trades_ (A) is an array of trade objects with the following fields for each trade:
    *   _seller_ (S) is the account number of the seller
    *   _quantityQNT_ (S) is the quantity (in QNT) of the asset traded
    *   _bidOrder_ (S) is the bid order ID
    *   _sellerRS_ (S) is the Reed-Solomon address of the seller
    *   _buyer_ (S) is the account number of the buyer
    *   _priceNQT_ (S) is the trade price (in NQT, the ask price for a buy or the bid price for a sell)
    *   _askOrder_ (S) is the ask order ID
    *   _buyerRS_ (S) is the Reed-Solomon address of the buyer
    *   _decimals_ (N) is the number of decimal places used by the asset
    *   _name_ (S) is the name of the asset (if _includeAssetInfo_ is _true_)
    *   _block_ (S) is the block ID of the trade (if _includeAssetInfo_ is _true_)
    *   _asset_ (S) is the asset ID
    *   _askOrderHeight_ (N) is the block height of the ask order
    *   _bidOrderHeight_ (N) is the block height of the bid order
    *   _tradeType_ (S) is the trade type (_sell_ or _buy_, where _buy_ implies that the bid occurred after the ask, or if in the same block, has a greater order ID)
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the trade block
    *   _height_ (N) is the height of the trade block
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Trades](API_Examples.md#get-trades "The Blue0x API Examples") example.

### Issue Asset

Create an asset on the exchange. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _issueAsset_
*   _name_ is the name of the asset
*   _description_ is a url-encoded description of the asset in UTF-8 with a maximum length of 1000 bytes (optional)
*   _quantityQNT_ is the total amount (in QNT) of the asset in existence
*   _decimals_ is the number of decimal places used by the asset (optional, zero default)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the asset ID.

**Example:** Refer to [Issue Asset](API_Examples.md#issue-asset "The Blue0x API Examples") example.

### Place Order

Place an asset order. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is either _placeBidOrder_ or _placeAskOrder_
*   _asset_ is the asset ID of the asset being ordered
*   _quantityQNT_ is the amount (in QNT) of the asset being ordered
*   _priceNQT_ is the bid/ask price (in NQT)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the order ID.

**Example:** Refer to [Place Order](API_Examples.md#place-order "The Blue0x API Examples") example.

#### Place Ask Order

Refer to [Place Order](#place-order "The Blue0x API").

#### Place Bid Order

Refer to [Place Order](#place-order "The Blue0x API").

### Search Assets

Get assets having a name or description that match a given query in reverse relevance order.

**Request:**

*   _requestType_ is _searchAssets_
*   _query_ is a full text query on the asset fields _name_ (S) and _description_ (S) in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _firstIndex_ is a zero-based index to the first asset to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last asset to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _assets_ (A) is an array of asset objects (refer to [Get Asset](#get-asset "The Blue0x API"))
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Assets](API_Examples.md#search-assets "The Blue0x API Examples") example.

### Transfer Asset

Transfer a quantity of an asset from one account to another. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _transferAsset_
*   _recipient_ is the recipient account ID
*   _recipientPublicKey_ is the public key of the recipient account (optional, enhances security of a new account)
*   _asset_ is the ID of the asset being transferred
*   _quantityQNT_ is the amount (in QNT) of the asset being transferred

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the transfered asset ID.

**Example:** Refer to [Transfer Asset](API_Examples.md#transfer-asset "The Blue0x API Examples") example.

Block Operations
-------------------

### Get Block

Get a block object given a block ID or block height.

**Request:**

*   _requestType_ is _getBlock_
*   _block_ is the block ID (optional)
*   _height_ is the block height (optional if _block_ provided)
*   _timestamp_ is the timestamp (in seconds since the genesis block) of the block (optional if _height_ provided)
*   _includeTransactions_ is _true_ to include transaction details (optional)
*   _includeExecutedPhased_ is _true_ to include approved and executed phased transactions (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** _block_ overrides _height_ which overrides _timestamp_.

**Response:**

*   _previousBlockHash_ (S) is the 32-byte hash of the previous block
*   _payloadLength_ (N) is the length (in bytes) of all transactions included in the block
*   _totalAmountNQT_ (S) is the total amount (in NQT) of the transactions in the block
*   _generationSignature_ (S) is the 32-byte generation signature of the generating account
*   _generator_ (S) is the generating account number
*   _generatorPublicKey_ (S) is the 32-byte public key of the generating account
*   _baseTarget_ (S) is the base target for the next block generation
*   _payloadHash_ (S) is the 32-byte hash of the payload (all transactions)
*   _generatorRS_ (S) is the Reed-Solomon address of the generating account
*   _nextBlock_ (S) is the next block ID
*   _numberOfTransactions_ (N) is the number of transactions in the block
*   _blockSignature_ (S) is the 64-byte block signature
*   _transactions_ (A) is an array of transaction IDs or transaction objects (if _includeTransactions_ provided, refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _executedPhasedTransactions_ (A) is an array of transaction IDs or transaction objects (if _includeExecutedPhased_ provided, refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _version_ (N) is the block version
*   _totalFeeNQT_ (S) is the total fee (in NQT) of the transactions in the block
*   _previousBlock_ (S) is the previous block ID
*   _cumulativeDifficulty_ (S) is the cumulative difficulty for the next block generation
*   _block_ (S) is the block ID
*   _height_ (N) is the zero-based block height
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the block
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Block](API_Examples.md#get-block "The Blue0x API Examples") example.

### Get Block Id

Get a block ID given a block height.

**Request:**

*   _requestType_ is _getBlockId_
*   _height_ is the block height
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _block_ (S) is the block ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Block Id](API_Examples.md#get-block-id "The Blue0x API Examples") example.

### Get Blocks

Get blocks from the blockchain in reverse block height order.

**Request:**

*   _requestType_ is _getBlocks_
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _firstIndex_ is first block to retrieve (optional, default is zero or the last block on the blockchain)
*   _lastIndex_ is the last block to retrieve (optional, default is _firstIndex_ + 99)
*   _includeTransactions_ is _true_ to include transaction details (optional)
*   _includeExecutedPhased_ is _true_ to include approved and executed phased transactions (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _blocks_ (A) is an array of blocks retrieved (refer to [Get Block](#get-block "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Blocks](API_Examples.md#get-blocks "The Blue0x API Examples") example.

### Get EC Block

Get Economic Cluster block data.

**Request:**

*   _requestType_ is _getECBlock_
*   _timestamp_ is the timestamp (in seconds since the genesis block) of the EC block (optional, default (or zero) is the current timestamp)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If _timestamp_ is more than 15 seconds before the timestamp of the last block on the blockchain, _errorCode_ 4 is returned.

**Response:**

*   _ecBlockHeight_ (N) is the EC block height
*   _ecBlockId_ (S) is the EC block ID
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the EC block
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get EC Block](API_Examples.md#get-ec-block "The Blue0x API Examples") example.

Digital Goods Store Operations
---------------------------------

In the BLX client interface, the Digital Goods Store (DGS) is referred to as the 'Marketplace'.

### DGS Delisting

Delist a listed product. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsDelisting_
*   _goods_ is the goods ID

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Delisting](API_Examples.md#dgs-delisting "The Blue0x API Examples") example.

### DGS Delivery

Deliver a product. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsDelivery_
*   _purchase_ is the purchase order ID
*   _discountNQT_ is a discount (in NQT) off the selling price (optional, default is zero)
*   _goodsToEncrypt_ is the product, a text or a hex string to be encrypted (optional if _goodsData_ provided)
*   _goodsIsText_ is _false_ if _goodsToEncrypt_ is a hex string (optional)
*   _goodsData_ is AES-encrypted (using [Encrypt To](#encrypt-to "The Blue0x API")) _goodsToEncrypt_, up to 1000 bytes long (required only if _secretPhrase_ is omitted)
*   _goodsNonce_ is the unique nonce associated with the encrypted data (required only if _secretPhrase_ is omitted)

**Note:** If the encrypted goods data is longer than 1000 bytes, use a prunable encrypted message to deliver the goods.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Delivery](API_Examples.md#dgs-delivery "The Blue0x API Examples") example.

### DGS Feedback

Give feedback about a purchased product after delivery. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsFeedback_
*   _purchase_ is the purchase order ID
*   _message_ is unencrypted (public) feedback text up to 1000 bytes

**Note**: The unencrypted _message_ parameter is used for public feedback.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Feedback](API_Examples.md#dgs-feedback "The Blue0x API Examples") example.

### DGS Listing

List a product in the DGS by creating a listing transaction. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsListing_
*   _name_ is the name of the product up to 100 characters in length
*   _description_ is a description of the product up to 1000 characters in length
*   _tags_ are up to three comma separated keywords describing the product up to 100 characters in length (optional)
*   _quantity_ is the quantity of the product for sale
*   _priceNQT_ is the price (in NQT) of the product

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the goods ID.

**Example:** Refer to [DGS Listing](API_Examples.md#dgs-listing "The Blue0x API Examples") example.

### DGS Price Change

Change the price of a listed product. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsPriceChange_
*   _goods_ is the goods ID of the product
*   _priceNQT_ is the new price of the product

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Price Change](API_Examples.md#dgs-price-change "The Blue0x API Examples") example.

### DGS Purchase

Purchase a product for sale. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsPurchase_
*   _goods_ is the goods ID of the product
*   _priceNQT_ is the price of the product
*   _quantity_ is the quantity to be purchased
*   _deliveryDeadlineTimestamp_ is the timestamp (in seconds since the genesis block) by which delivery of the product must occur

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the purchase order ID.

**Example:** Refer to [DGS Purchase](API_Examples.md#dgs-purchase "The Blue0x API Examples") example.

### DGS Quantity Change

Change the quantity of a listed product. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsQuantityChange_
*   _goods_ is the goods ID of the product
*   _deltaQuantity_ is the change in the quantity of the product for sale (use negative numbers for a decrease in quantity)

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Quantity Change](API_Examples.md#dgs_Quantity-change "The Blue0x API Examples") example.

### DGS Refund

Refund a purchase. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _dgsRefund_
*   _purchase_ is the purchase order ID
*   _refundNQT_ is the amount (in NQT) of the refund

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [DGS Refund](API_Examples.md#dgs-refund "The Blue0x API Examples") example.

### Get DGS Expired Purchases

Get purchase orders which have expired without being delivered, given a seller ID, in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSExpiredPurchases_
*   _seller_ is the account ID of the product seller
*   _firstIndex_ is a zero-based index to the first purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last purchase order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Expired Purchases](API_Examples.md#get-dgs-expired-purchases "The Blue0x API Examples") example.

### Get DGS Good

Get a DGS product given a goods ID.

**Request:**

*   _requestType_ is _getDGSGood_
*   _goods_ is the goods ID of the product
*   _includeCounts_ is _true_ if the fields beginning with numberOf... are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _seller_ (S) is the seller's account ID
*   _quantity_ (N) is the quantity of the product remaining for sale
*   _goods_ (S) is the ID of the product
*   _description_ (S) is the description of the product
*   _sellerRS_ (S) is the Reed-Solomon address of the seller's account
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _delisted_ (B) is _true_ if the product has been delisted, _false_ otherwise
*   _parsedTags_ (A) is an array of up to three tag strings, parsed from the _tags_ field
*   _tags_ (S) is the comma separated list of tags provided by the seller when the listing was created
*   _priceNQT_ (S) is the current price of the product
*   _numberOfPublicFeedbacks_ (N) is the number of public feedbacks given for the product
*   _name_ (S) is the name of the product
*   _numberOfPurchases_ (N) is the number of purchases of the product
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the creation of the product listing
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)

**Example:** Refer to [Get DGS Good](API_Examples.md#get-dgs-good "The Blue0x API Examples") example.

### Get DGS Goods

Get DGS products for sale in reverse chronological listing creation order unless a seller is given, then in product name order.

**Request:**

*   _requestType_ is _getDGSGoods_
*   _seller_ is the account ID of the product seller (optional)
*   _firstIndex_ is a zero-based index to the first product to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last product to retrieve (optional)
*   _inStockOnly_ is _false_ if out-of-stock products (zero quantity) are to be retrieved (optional)
*   _hideDelisted_ is _true_ if delisted products are to be omitted (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** If none of the optional parameters are specified, all in-stock products in the blockchain are retrieved at once, which may take a long time.

**Response:**

*   _goods_ (A) is an array of goods (refer to [Get DGS Good](#get-dgs-good "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Goods](API_Examples.md#get-dgs-goods "The Blue0x API Examples") example.

### Get DGS Goods Count

Get the number of products for sale by a given seller or all sellers.

**Request:**

*   _requestType_ is _getDGSGoodsCount_
*   _seller_ is the account ID of the seller (optional, default is all sellers combined)
*   _inStockOnly_ is _false_ if out-of-stock (zero quantity) products are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfGoods_ (N) is the number of goods for sale by the _seller_
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _numberOfGoods_ field refers to the number of distinct products for sale, regardless of the quantity of each.

**Example:** Refer to [Get DGS Goods Count](API_Examples.md#get-dgs-goods-count "The Blue0x API Examples") example.

### Get DGS Goods Purchase Count

Get the number of completed purchase orders given a goods ID.

**Request:**

*   _requestType_ is _getDGSGoodsPurchaseCount_
*   _goods_ is the goods ID
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfPurchases_ (N) is the number of completed purchase orders
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Goods Purchase Count](API_Examples.md#get-dgs-goods-purchase-count "The Blue0x API Examples") example.

### Get DGS Goods Purchases

Get completed purchase orders given a goods ID and optionally a buyer ID in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSGoodsPurchases_
*   _goods_ is the goods ID
*   _buyer_ is a buyer ID (optional)
*   _firstIndex_ is a zero-based index to the first purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last purchase order to retrieve (optional)
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Goods Purchases](API_Examples.md#get-dgs-goods-purchases "The Blue0x API Examples") example.

### Get DGS Pending Purchases

Get pending purchase orders given a seller ID in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSPendingPurchases_
*   _seller_ is the account ID of the seller
*   _firstIndex_ is a zero-based index to the first purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last purchase order to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of pending purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Pending Purchases](API_Examples.md#get-dgs-pending-purchases "The Blue0x API Examples") example.

### Get DGS Purchase

Get a purchase order given a purchase order ID.

**Request:**

*   _requestType_ is _getDGSPurchase_
*   _purchase_ is the purchase order ID
*   _sharedKey_ is the shared key used to decrypt the message (optional) (see [Get Shared Key](#get-shared-key "The Blue0x API"))
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _seller_ (S) is the account number of the seller
*   _quantity_ (N) is the quantity of the product to be purchased
*   _feedbackNotes_ (A) is an array of AES-encrypted objects, each with _data_ (S) and _nonce_ (S) fields, in reverse chronological order, if applicable
*   _publicFeedbacks_ (A) is an array of feedback strings in reverse chronological order if applicable
*   _pending_ (B) is _true_ if the _deliveryDeadline_ has not passed, _false_ otherwise
*   _purchase_ (S) is the purchase order ID
*   _goods_ (S) is the ID of the product
*   _sellerRS_ (S) is the Reed-Solomon address of the seller
*   _buyer_ (S) is the account number of the buyer
*   _priceNQT_ (S) is the price (in NQT) of the product
*   _deliveryDeadlineTimestamp_ (N) is the timestamp (in seconds since the genesis block) by which the product must be delivered
*   _goodsIsText_ (B) is _false_ if the message is a hex string, otherwise the message is text (optional)
*   _buyerRS_ (S) is the Reed-Solomon address of the buyer
*   _refundNQT_ (S) is the amount (in NQT) refunded, if applicable
*   _name_ (S) is the name of the product
*   _goodsData_ (O) is an object with the two fields _data_ (S) (the encrypted product hex string) and _nonce_ (S), if the product has been delivered
*   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the purchase order
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Purchase](API_Examples.md#get-dgs-purchase "The Blue0x API Examples") example.

### Get DGS Purchase Count

Get the number of purchase orders given a seller and/or buyer ID, or all orders combined.

**Request:**

*   _requestType_ is _getDGSPurchaseCount_
*   _seller_ is the account ID of the seller (optional, default is all sellers)
*   _buyer_ is the account ID of the buyer (optional, default is all buyers)
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfPurchases_ (N) is the number of purchase orders associated with the seller and/or buyer
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Purchase Count](API_Examples.md#get-dgs-purchase-count "The Blue0x API Examples") example.

### Get DGS Purchases

Get purchase orders given a seller and/or buyer ID in reverse chronological order.

**Request:**

*   _requestType_ is _getDGSPurchases_
*   _seller_ is the account ID of the seller (optional)
*   _buyer_ is the account ID of the buyer (optional if _seller_ provided)
*   _firstIndex_ is a zero-based index to the purchase order to retrieve (optional)
*   _lastIndex_ is a zero-based index to the purchase order to retrieve (optional)
*   _withPublicFeedbacksOnly_ is _true_ if purchase orders without public feedback are to be omitted (optional)
*   _completed_ is _true_ if only completed purchase orders are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _purchases_ (A) is an array of purchase orders (refer to [Get DGS Purchase](#get-dgs-purchase "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Purchases](API_Examples.md#get-dgs-purchases "The Blue0x API Examples") example.

### Get DGS Tag Count

Get the number of tags used by all sellers.

**Request:**

*   _requestType_ is _getDGSTagCount_
*   _inStockOnly_ is _false_ if tags with no associated in-stock products are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfTags_ (N) is the number of tags
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get DGS Tag Count](API_Examples.md#get-dgs-tag-count "The Blue0x API Examples") example.

### Get DGS Tags

Get tags used by all sellers in reverse _inStockCount_, reverse _totalCount_, _tag_ order.

**Request:**

*   _requestType_ is _getDGSTags_
*   _inStockOnly_ is _false_ if out-of-stock tags are to be retrieved (optional)
*   _firstIndex_ is a zero-based index to the first tag to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tag to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _tags_ (A) is an array of tag objects with the following fields for each tag:
    *   _inStockCount_ (N) is the number of products available for sale as tagged
    *   _tag_ (S) is the tag word
    *   _totalCount_ (N) is the total number of products as tagged
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _...Count_ fields refer to the number of distinct products tagged, regardless of the quantity of each.

**Example:** Refer to [Get DGS Tags](API_Examples.md#get-dgs-tags "The Blue0x API Examples") example.

### Get DGS Tags Like

Get all tags starting with a given prefix (at least 2 characters long) in reverse _inStockCount_, reverse _totalCount_, _tag_ order.

**Request:**

*   _requestType_ is _getDGSTagsLike_
*   _tagPrefix_ is the prefix (at least 2 characters long) of the _tag_
*   _inStockOnly_ is _false_ if out-of-stock tags are to be retrieved (optional)
*   _firstIndex_ is a zero-based index to the first tag to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last tag to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _tags_ (A) is an array of tag objects with the following fields for each tag:
    *   _inStockCount_ (N) is the number of products available for sale as tagged
    *   _tag_ (S) is the tag word
    *   _totalCount_ (N) is the total number of products as tagged
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _...Count_ fields refer to the number of distinct products tagged, regardless of the quantity of each.

**Example:** Refer to [Get DGS Tags Like](API_Examples.md#get-dgs-tags-like "The Blue0x API Examples") example.

### Search DGS Goods

Get product listings that have a name or description that match a given query in reverse relevance order, then name order (given a seller), then reverse chronological order.

**Request:**

*   _requestType_ is _searchDGSGoods_
*   _query_ is a full text query on the goods fields _name_ and _description_ in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview) (optional)
*   _tag_ is a query on the good field _tags_ in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview) (optional)
*   _seller_ is the account ID of the product seller (optional)
*   _firstIndex_ is a zero-based index to the first product to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last product to retrieve (optional)
*   _inStockOnly_ is _false_ if out-of-stock products (zero quantity) are to be retrieved (optional)
*   _hideDelisted_ is _true_ if delisted products are to be omitted (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _goods_ (A) is an array of goods objects (refer to [Get DGS Good](#get-dgs-good "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search DGS Goods](API_Examples.md#search-dgs-goods "The Blue0x API Examples") example.

Forging Operations
---------------------

### Start / Stop / Get Forging

Start or stop forging with an account, or check to see if an account is forging. POST only.

**Request:**

*   _requestType_ is either _startForging_, _stopForging_ or _getForging_
*   _secretPhrase_ is the secret passphrase of the account (optional for _stopForging_ and _getForging_ if password protected like the [Debug Operations](#debug-operations "The Blue0x API"))

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

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _leaseBalance_
*   _period_ is the lease period (in number of blocks, 1440 minimum)
*   _recipient_ is the lessee (recipient) account
*   _recipientPublicKey_ is the public key of the lessee (recipient) account (optional, enhances security of a new account)

**Response:** Refer to [Create Transaction Response](#create-transaction-response "The Blue0x API").

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

### Mark Host

Generates a node hallmark. POST only.

**Request:**

*   _requestType_ is _markHost_
*   _secretPhrase_ is the secret passphrase for the account that will be hallmarked on the node
*   _host_ is the IP address or domain name of the node
*   _weight_ is the weight to assign to the node
*   _date_ is the current date in YYYY-MM-DD format

**Note:** Refer to [Create Hallmark](https://nxtwiki.org/wiki/How-To:CreateHallmark "How-To:CreateHallmark") for details.

**Response:**

*   _hallmark_ (S) is the hallmark hex string
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Refer to [Create Hallmark](https://nxtwiki.org/wiki/How-To:CreateHallmark "How-To:CreateHallmark") for instructions for applying the hallmark to a public node.

**Example:** Refer to [Mark Host](API_Examples.md#mark-host "The Blue0x API Examples") example.

#### Generate Hallmark

Refer to [Mark Host](#mark-host "The Blue0x API").

Monetary System Operations
-----------------------------

### Can Delete Currency

Determine if a currency can be deleted.

**Request:**

*   _requestType_ is _canDeleteCurrency_
*   _account_ is the account ID
*   _currency_ is the currency ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _canDelete_ (B) is _true_ if the currency can be deleted, _false_ otherwise
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** a currency can be deleted only when all units of the currency are held by _account_. A reservable currency that has not yet been issued can be deleted by the issuer. A mintable currency that has not completed minting cannot be deleted by a non-issuer.

**Example:** Refer to [Can Delete Currency](API_Examples.md#can-delete-currency "The Blue0x API Examples") example.

### Currency Buy / Sell

Make an exchange request to buy or sell an exchangeable currency. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyBuy_ or _currencySell_
*   _currency_ is the currency ID
*   _rateNQT_ is the exchange rate (in NQT per QNT)
*   _units_ is the amount of the currency to buy or sell (in QNT)

**Note:** An exchange request is immediately executed once accepted onto the blockchain based only on currently available offers (refer to [Publish Exchange Offer](#publish-exchange-offer "The Blue0x API")). The request then expires, regardless of the amount of currency exchanged; the request may be completely filled, partially filled, or expire without any exchange if no matching offers are found.

**Response:** Refer to [Create Transaction Response](#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Buy / Sell](API_Examples.md#currency-buy-sell "The Blue0x API Examples") example.

#### Currency Buy

Refer to [Currency Buy / Sell](#currency-buy-sell "The Blue0x API").

#### Currency Sell

Refer to [Currency Buy / Sell](#currency-buy-sell "The Blue0x API").

### Currency Mint

Submit a valid computed nonce to the blockchain in return for newly minted currency. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyMint_
*   _currency_ is the mintable currency ID
*   _nonce_ is the computed nonce
*   _units_ is the amount (in QNT) of currency to mint
*   _counter_ (N) is the counter associated with the minting account

**Note:** The hash of _nonce_ must be less than _targetBytes_ provided by [Get Minting Target](#get-minting-target "The Blue0x API") for given _units_ and _counter_. _counter_ must be increased with each submission.

**Response:** Refer to [Create Transaction Response](#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Mint](API_Examples.md#currency-mint "The Blue0x API Examples") example.

### Currency Reserve Claim

Claim currency reserve. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyReserveClaim_
*   _currency_ is the currency ID
*   _units_ is the amount (in QNT) of reserve currency to claim

**Note:** Holders of a claimable currency may claim the locked NQT backing their units, thus reducing the supply of the currency.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Reserve Claim](API_Examples.md#currency-reserve-claim "The Blue0x API Examples") example.

### Currency Reserve Increase

Increase the currency reserve of an unissued reservable currency. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _currencyReserveIncrease_
*   _currency_ is the currency ID
*   _amountPerUnitNQT_ is the additional amount (in NQT per QNT of _reserveSupply_) to reserve (refer to [Issue Currency](#issue-currency "The Blue0x API"))

**Note:** An additional _amountPerUnitNQT_ \* _reserveSupply_ NQT (beyond what has previously been reserved) will be locked until the _issuanceHeight_ is reached. Upon issuance, if the currency is claimable the NQT will remain locked until claimed; otherwise the NQT will transfer to the issuing account. Also upon issuance, a portion of the _reserveSupply_ QNT will be transfered to each reserving account in proportion to the NQT that was contributed.

**Response:** Refer to [Create Transaction Response](#create-transaction-response "The Blue0x API").

**Example:** Refer to [Currency Reserve Increase](API_Examples.md#currency-reserve-increase "The Blue0x API Examples") example.

### Delete Currency

Delete a deletable currency (refer to [Can Delete Currency](#can-delete-currency "The Blue0x API")). POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _deleteCurrency_
*   _currency_ is the currency ID

**Response:** Refer to [Create Transaction Response](#create-transaction-response "The Blue0x API").

**Example:** Refer to [Delete Currency](API_Examples.md#delete-currency "The Blue0x API Examples") example.

### Get Account Currencies

Get the currencies issued by a given account.

**Request:**

*   _requestType_ is _getAccountCurrencies_
*   _account_ is the account ID
*   _currency_ is a currency ID filter (optional)
*   _height_ is the blockchain height at which the response applies (optional, default is the current height)
*   _includeCurrencyInfo_ is _true_ if several currency information properties is to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _accountCurrencies_ (A) is an array of currency objects with the following fields:
    *   _code_ (S) is the currency code
    *   _unconfirmedUnits_ (S) is the amount of unconfirmed currency units (in QNT)
    *   _decimals_ (N) is the number of currency decimal places
    *   _name_ (S) is the currency name
    *   _currency_ (S) is the currency ID
    *   _units_ (S) is the amount of currency (in QNT)
    *   _issuanceHeight_ (N) is the blockchain height of issuance for a reservable currency
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API"))
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Currencies](API_Examples.md#get-account-currencies "The Blue0x API Examples") example.

### Get Account Currency Count

Get the number of currencies issued by a given account.

**Request:**

*   _requestType_ is _getAccountCurrencyCount_
*   _account_ is the account ID
*   _height_ is the blockchain height at which the response applies (optional, default is the current height)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfCurrencies_ (N) is the number of currencies issued
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Currency Count](API_Examples.md#get-account-currency-count "The Blue0x API Examples") example.

### Get Account Exchange Requests

Get the exchange requests associated with a given account and/or currency in reverse chronological order (or in expected order of execution for expected requests).

**Request:**

*   _requestType_ is either _getAccountExchangeRequests_ or _getExpectedExchangeRequests_, where expected requests are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _account_ is the account ID (optional for expected requests)
*   _currency_ is the currency ID (optional for expected requests if _account_ provided)
*   _includeCurrencyInfo_ is _true_ to include the response fields _code_, _decimals_, _name_, _issuanceHeight_, _type_, _timestamp_, _issuerAccountRS_ and _issuerAccount_ (optional, applies only to expected requests)
*   _firstIndex_ is a zero-based index to the first request to retrieve (optional, does not apply to expected requests)
*   _lastIndex_ is a zero-based index to the last request to retrieve (optional, does not apply to expected requests)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchangeRequests_ (A) is an array of requests with the following fields for each request:
    *   _code_ (S) is a currency code
    *   _subtype_ (N) is _5_ for buy and _6_ for sell
    *   _decimals_ (N) is the number of decimal places
    *   _name_ (S) is the currency name
    *   _units_ (S) is the number of currency units to buy or sell (in QNT)
    *   _issuanceHeight_ (N) is the blockchain height of issuance for a reservable currency, zero otherwise
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API"))
    *   _transaction_ (S) is the transaction ID
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the block when the request was executed
    *   _rateNQT_ (S) is the exchange rate (in NQT per QNT)
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
    *   _phased_ (B) is true if the transaction is phased (applies only to expected requests)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Account Exchange Requests](API_Examples.md#get-account-exchange-requests "The Blue0x API Examples") example.

#### Get Expected Exchange Requests

Refer to [Get Account Exchange Requests](#get-account-exchange-requests "The Blue0x API").

### Get Funding Monitor

Get a funding monitor.

**Request:**

*   _requestType_ is _getFundingMonitor_
*   _secretPhrase_ is the secret phrase of the funding account, used to get a single monitor. (optional)
*   _adminPassword_ is the admin password, used to get a single monitor or all monitors (optional if _secretPhrase_ is provided)
*   _includeMonitoredAccounts_ is _true_ to include account info of the monitored accounts (optional)
*   _property_ is the name of the account property (optional)
*   _holdingType_ is a string representing the holding type (optional)
*   _holding_ is the holding ID (optional)
*   _account_ is the account ID (optional)

**Response:**

*   _monitors_ (A) is an array of monitor objects including the following fields:
    *   _holding_ (S) is the holding ID
    *   _amount_ (S) is the amount to fund the monitored accounts with (NQT or QNT)
    *   _monitoredAccounts_ (A) is an array of monitored account objects including the following fields (only if _includeMonitoredAccounts_ is _true_):
        *   _amount_ (S) is the amount to fund the monitored accounts with. Overrides amount in parent object.
        *   _account_ (S) is the monitored account ID
        *   _accountRS_ (S) is the monitored account Reed Solomon address
        *   _threshold_ (S) is the threshold. Overrides threshold in parent object.
        *   _interval_ (N) is the interval. Overrides interval in parent object.
    *   _holdingType_ (N) is the holding type
    *   _account_ (S) is the monitoring account ID
    *   _accountRS_ (S) is the Reed Solomon address of the monitoring account
    *   _property_ (S) is the account property
    *   _threshold_ (S) is the threshold
    *   _interval_ (N) is the interval
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Funding Monitor](API_Examples.md#get-funding-monitor "The Blue0x API Examples") example.

### Get All Currencies

Get all currencies in reverse creation order.

**Request:**

*   _requestType_ is _getAllCurrencies_
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _includeCounts_ is _true_ to include _numberOf..._ fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of currency objects (refer to [Get Currency](#get-currency "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Currencies](API_Examples.md#get-all-currencies "The Blue0x API Examples") example.

### Get All Exchanges

Get all currency exchanges in reverse chronological order.

**Request:**

*   _requestType_ is _getAllExchanges_
*   _timestamp_ is the earliest timestamp to retrieve (optional)
*   _firstIndex_ is a zero-based index to the first exchange to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last exchange to retrieve (optional)
*   _includeCurrencyInfo_ is _true_ to include some currency details (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Exchanges](API_Examples.md#get-all-exchanges "The Blue0x API Examples") example.

### Get Available To Buy

Calculates the rate required in order to completely fill an exchange request.

**Request:**

*   _requestType_ is _getAvailableToBuy_ or _getAvailableToSell_
*   _currency_ is the currency ID
*   _units_ is the number of units to buy
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _amountNQT_ (S) is the total amount needed to fill the exchange request
*   _units_ (S) is the number of units
*   _rateNQT_ (S) is the rate for the currency units
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Available To Buy](API_Examples.md#get-available-to-buy "The Blue0x API Examples") example.

#### Get Available To Sell

Refer to [Get Available To Buy](#get-available-to-buy "The Blue0x API").

### Get Buy / Sell Offers

Get currency buy or sell offers given a currency ID and/or an account ID in order of rate (if _sortByRate_ is _true_ for expected offers, otherwise in the expected order of execution).

**Request:**

*   _requestType_ is one of _getBuyOffers_, _getSellOffers_, _getExpectedBuyOffers_ or _getExpectedSellOffers_, where expected offers are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _currency_ is the currency ID (optional)
*   _account_ is the account ID (optional if _currency_ provided)
*   _availableOnly_ is _true_ to include only offers with non-zero supply and limit, but is ignored when both _currency_ and _account_ are given (optional, does not apply to expected offers)
*   _sortByRate_ is _true_ to sort by rate (optional, applies only to expected offers, which are returned in expected order of execution by default)
*   _firstIndex_ is a zero-based index to the first offer to retrieve (optional, does not apply to expected offers)
*   _lastIndex_ is a zero-based index to the last offer to retrieve (optional, does not apply to expected offers)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _offers_ (A) is an array of buy or sell offer objects (refer to [Get Offer](#get-offer "The Blue0x API") for details) with the following additional field only for an expected offer:
    *   _phased_ (B) is _true_ if the offer is phased, _false_ otherwise
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Buy / Sell Offers](API_Examples.md#get-buy-sell-offers "The Blue0x API Examples") example.

#### Get Buy Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

#### Get Sell Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

#### Get Expected Buy Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

#### Get Expected Sell Offers

Refer to [Get Buy / Sell Offers](#get-buy-sell-offers "The Blue0x API").

### Get Currencies

Get currencies given multiple currency IDs.

**Request:**

*   _requestType_ is _getCurrencies_
*   _currencies_ is one of multiple currency IDs
*   _currencies_ is one of multiple currency IDs

⋮

*   _includeCounts_ is _true_ to include _numberOf..._ fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of currency objects (refer to [Get Currency](#get-currency "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currencies](API_Examples.md#get-currencies "The Blue0x API Examples") example.

### Get Currencies By Issuer

Get currencies issued by multiple accounts in reverse creation order.

**Request:**

*   _requestType_ is _getCurrenciesByIssuer_
*   _account_ is one of multiple account IDs
*   _account_ is one of multiple account IDs
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _includeCounts_ is _true_ if _numberOf..._ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of arrays of currency objects, where the outer array is indexed by the given account IDs (refer to [Get Currency](#get-currency "The Blue0x API") for details about the currency objects)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currencies By Issuer](API_Examples.md#get-currencies-by-issuer "The Blue0x API Examples") example.

### Get Currency

Get the details of a currency.

**Request:**

*   _requestType_ is _getCurrency_
*   _currency_ is the currency ID (optional)
*   _code_ is the currency code (optional if _currency_ provided)
*   _includeCounts_ is _true_ if _numberOf..._ fields are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _initialSupply_ (S) is the initial currency supply (in QNT)
*   _currentReservePerUnitNQT_ (S) is the minimum currency reserve (in NQT per QNT)
*   _types_ (A) is an array of currency types, one or more of:
    *   -eXCHANGEABLE_
    *   -cONTROLLABLE_
    *   -rESERVABLE_
    *   -cLAIMABLE_
    *   -mINTABLE_
    *   _NON\-sHUFFLEABLE_
*   _code_ (S) is the currency code
*   _creationHeight_ (N) is the blockchain height of the currency creation
*   _minDifficulty_ (N) is the minimum difficulty for a mintable currency
*   _numberOfTransfers_ (N) is the number of currency transfers
*   _description_ (S) is the currency description
*   _minReservePerUnitNQT_ (S) is the minimum currency reserve (in NQT per QNT) for a reservable currency
*   _currentSupply_ (S) is the current currency supply (in QNT)
*   _issuanceHeight_ (N) is the blockchain height of the currency issuance for a reservable currency
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _type_ (N) is the currency type bitmask, from least to most significant bit: exchangeable, controllable, reservable, claimable, mintable, non-shuffleable
*   _reserveSupply_ (S) is the reserve currency supply (in NQT) for a reservable currency
*   _maxDifficulty_ (N) is the maximum difficulty for a mintable currency
*   _accountRS_ (S) is the Reed-Solomon address of the issuing account
*   _decimals_ (N) is the number of decimal places used by the currency
*   _name_ (S) is the name of the currency
*   _numberOfExchanges_ (N) is the number of currency exchanges
*   _currency_ (S) is the currency ID
*   _maxSupply_ (S) is the maximum currency supply (in QNT)
*   _account_ (S) is the account ID of the currency issuer
*   _algorithm_ (N) is the algorithm number for a mintable currency: 2 for SHA256, 3 for SHA3, 5 for Scrypt, 25 for Keccak25
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)

**Example:** Refer to [Get Currency](API_Examples.md#get-currency "The Blue0x API Examples") example.

### Get Currency Account Count

Get the number of accounts that hold a given currency.

**Request:**

*   _requestType_ is _getCurrencyAccountCount_
*   _currency_ is the currency ID
*   _height_ is the blockchain height at which the response applies (optional, default is the current height)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _numberOfAccounts_ (N) is the number of accounts that hold the currency
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Account Count](API_Examples.md#get-currency-account-count "The Blue0x API Examples") example.

### Get Currency Accounts

Get the accounts that hold a given currency in reverse units order.

**Request:**

*   _requestType_ is _getCurrencyAccounts_
*   _currency_ is the currency ID
*   _height_ is the blockchain height at which the response applies (optional, default is current height)
*   _firstIndex_ is a zero-based index to the first account to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last account to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _accountCurrencies_ (A) is an array of account objects with the following fields:
    *   _unconfirmedUnits_ (S) is the amount of unconfirmed currency units (in QNT)
    *   _accountRS_ (S) is the Reed-Solomon address of the account
    *   _currency_ (S) is the currency ID
    *   _units_ (S) is the amount of currency (in QNT)
    *   _account_ (S) is the account number
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Accounts](API_Examples.md#get-currency-accounts "The Blue0x API Examples") example.

### Get Currency Founders

Get a reservable currency's founders.

**Request:**

*   _requestType_ is _getCurrencyFounders_
*   _currency_ is the currency ID
*   _account_ is an account ID (optional)
*   _firstIndex_ is a zero-based index to the first founder to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last founder to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _founders_ (A) is an array of founder objects each of which has the following fields:
    *   _accountRS_ (S) is the Reed-Solomon address of the founding account
    *   _amountPerUnitNQT_ (S) is the amount (in NQT per QNT of _reserveSupply_) reserved by the founder
    *   _currency_ (S) is the currency ID
    *   _account_ (S) is the founding account number
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Founders](API_Examples.md#get-currency-founders "The Blue0x API Examples") example.

### Get Currency Ids

Get all currency IDs in reverse chronological creation order.

**Request:**

*   _requestType_ is _getCurrencyIds_
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencyIds_(A) is an array of currency IDs
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Ids](API_Examples.md#get-currency-ids "The Blue0x API Examples") example.

### Get Currency Transfers

Get currency transfers given a currency ID and/or an account ID in reverse block height order (or in expected order of execution for expected transfers).

**Request:**

*   _requestType_ is either _getCurrencyTransfers_ or _getExpectedCurrencyTransfers_, where expected transfers are from the unconfirmed transactions pool or are phased transactions scheduled to finish in the next block
*   _currency_ is the currency ID (optional)
*   _account_ is the account ID (optional if _currency_ provided)
*   _timestamp_ is the earliest transfer (in seconds since the genesis block) to retrieve (optional, does not apply to expected transfers)
*   _firstIndex_ is a zero-based index to the first transfer to retrieve (optional, does not apply to expected transfers)
*   _lastIndex_ is a zero-based index to the last transfer to retrieve (optional, does not apply to expected transfers)
*   _includeCurrencyInfo_ is _true_ to include some currency fields (optional, does not apply to expected transfers)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transfers_ (A) is an array of transfer objects with the following fields for each transfer:
    *   _code_ (S) is the currency code
    *   _units_ (S) is the amount (in QNT) of the transfer
    *   _issuanceHeight_ (N) is the blockchain height of the currency issuance for a reservable currency
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API") for details)
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _transfer_ (S) is the transfer ID
    *   _senderRS_ (S) is the Reed-Solomon address of the sender account
    *   _sender_ (S) is the account number of the sender account
    *   _recipientRS_ (S) is the Reed-Solomon address of the recipient account
    *   _decimals_ (N) is the number of decimal places used by the currency
    *   _recipient_ (S) is the account number of the recipient account
    *   _name_ (S) is the currency name
    *   _currency_ (S) is the currency ID
    *   _issuerAccount_ (S) is the issuer account ID
    *   _height_ (N) is the blockchain height of the transfer
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the transfer block, does not apply to an expected transfer
    *   _phased_ (B) is _true_ if the transaction is phased, _false_ otherwise, applies only to an expected transfer
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Currency Transfers](API_Examples.md#get-currency-transfers "The Blue0x API Examples") example.

#### Get Expected Currency Transfers

Refer to [Get Currency Transfers](#get-currency-transfers "The Blue0x API").

### Get Exchanges

Get currency exchanges given a currency and/or an account in reverse chronological order.

**Request:**

*   _requestType_ is _getExchanges_
*   _currency_ is a currency ID (optional)
*   _account_ is an account ID (optional if _currency_ provided)
*   _firstIndex_ is a zero-based index to the first currency exchange to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency exchange to retrieve (optional)
*   _timestamp_ is the earliest block (in seconds since the genesis block) to retrieve (optional)
*   _includeCurrencyInfo_ is _true_ to include several currency-related fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects with the following fields for each exchange:
    *   _seller_ (S) is the seller account number
    *   _code_ (S) is the currency code
    *   _sellerRS_ (S) is the Reed-Solomon address of the seller account
    *   _units_ (S) is the amount of currency exchanged (in QNT)
    *   _issuanceHeight_ (N) is the blockchain height of currency issuance for a reservable currency
    *   _type_ (N) is the currency type bitmask (refer to [Get Currency](#get-currency "The Blue0x API") for details)
    *   _rateNQT_ (S) is the currency exchange rate (in NQT per QNT)
    *   _buyer_ (S) is the account number of the buyer
    *   _offer_ (S) is the offer ID
    *   _buyerRS_ (S) is the Reed-Solomon address of the buyer account
    *   _decimals_ (N) is the number of decimal places used by the currency
    *   _name_ (S) is the currency name
    *   _currency_ (S) is the currency ID
    *   _block_ (S) is the block ID of the block containing the exchange transaction
    *   _transaction_ (S) is the transaction ID of the exchange
    *   _timestamp_ (N) is the timestamp (in seconds since the genesis block) of the exchange
    *   _height_ is the blockchain height of the block containing the exchange transaction
    *   _issuerAccountRS_ (S) is the Reed-Solomon address of the issuer account
    *   _issuerAccount_ (S) is the issuer account ID
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Exchanges](API_Examples.md#get-exchanges "The Blue0x API Examples") example.

### Get Exchanges By Exchange Request

Get currency exchanges given an exchange request transaction ID in reverse chronological order.

**Request:**

*   _requestType_ is _getExchangesByExchangeRequest_
*   _transaction_ is the transaction ID of the exchange request
*   _includeCurrencyInfo_ is _true_ to include several currency-related fields (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Exchanges By Exchange Request](API_Examples.md#get-exchanges-by-exchange-request "The Blue0x API Examples") example.

### Get Exchanges By Offer

Get currency exchanges given a currency offer ID in reverse chronological order.

**Request:**

*   _requestType_ is _getExchangesByOffer_
*   _offer_ (S) is a currency offer ID
*   _includeCurrencyInfo_ is _true_ to include several currency-related fields (optional)
*   _firstIndex_ is a zero-based index to the first currency exchange to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency exchange to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Exchanges By Offer](API_Examples.md#get-exchanges-by-offer "The Blue0x API Examples") example.

### Get Last Exchanges

Get the last exchange of each of multiple currencies.

**Request:**

*   _requestType_ is _getLastExchanges_
*   _currencies_ is one of multiple currency IDs
*   _currencies_ is one of multiple currency IDs

⋮

*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _exchanges_ (A) is an array of exchange objects (refer to [Get Exchanges](#get-exchanges "The Blue0x API") without _name_, _decimals_, _code_, _issuanceHeight_, _type_, _issuerAccountRS_ and _issuerAccount_ for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Last Exchanges](API_Examples.md#get_last-exchanges "The Blue0x API Examples") example.

### Get Minting Target

Get the current minting target of a mintable currency.

**Request:**

*   _requestType_ is _getMintingTarget_
*   _currency_ is the mintable currency ID
*   _account_ is the minting account ID
*   _units_ is the amount (in QNT) of currency to mint
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Note:** _units_ cannot be greater than 1/10000 of the _maxSupply_ (refer to [Issue Currency](#issue-currency "The Blue0x API")). Increasing _units_ decreases _targetBytes_.

**Response:**

*   _difficulty_ (S) is the current difficulty, an estimate of the number of hashes needed to meet the target
*   _targetBytes_ (S) is the 32-byte target (little endian), which must equal or exceed the computed hash of the _nonce_
*   _currency_ (S) is the currency ID
*   _counter_ (N) is the counter associated with the minting account, the value previously submitted to [Currency Mint](#currency-mint "The Blue0x API")
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** If a _nonce_ is found such that its hash is less than the target, it can be submitted to the blockchain along with _counter_ + 1 using [Currency Mint](#currency-mint "The Blue0x API"), which results in _units_ NQT being credited to the minting account. _difficulty_ is inversely related to the target, and so increases exponentially as _maxSupply_ is approached because the target is defined as (2exp\-1)/_units_, where exp decreases linearly from 256-_minDifficulty_ to 256-_maxDifficulty_. (Refer to [Issue Currency](#issue-currency "The Blue0x API") for _maxSupply_, _minDifficulty_ and _maxDifficulty_.)

**Example:** Refer to [Get Minting Target](API_Examples.md#get-minting-target "The Blue0x API Examples") example.

### Get Offer

Get offer details given an offer ID.

**Request:**

*   _requestType_ is _getOffer_
*   _offer_ is the offer ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _sellOffer_ and _buyOffer_ (O) are objects with the following fields:
    *   _offer_ (S) is the offer ID
    *   _expirationHeight_ (N) is the blockchain height of offer expiration
    *   _accountRS_ (S) is the Reed-Solomon address of the offering account
    *   -limit_ (S) is the cumulative limit of currency buys or sells
    *   _currency_ (S) is the currency ID
    *   _supply_ (S) is the current currency supply
    *   _account_ (S) is the offering account number
    *   _height_ (N) is the blockchain height of offer creation
    *   _rateNQT_ (S) is the currency exchange rate (in NQT per QNT)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Offer](API_Examples.md#get-offer "The Blue0x API Examples") example.

### Issue Currency

Issue a new currency or re-issue an existing currency with different properties. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _issueCurrency_
*   _name_ is the currency name, 3 to 10 characters and longer than the currency code
*   _code_ is the uppercase currency code with the following fee structure: three letters 25000 BLX, four letters 1000 BLX, five letters 40 BLX, re-issue 40 BLX
*   _description_ is the currency description
*   _type_ is the currency type bitmask, from least to most significant bit: exchangeable, controllable, reservable, claimable, mintable, non-shuffleable
*   _initialSupply_ is the initial currency supply (in QNT) (must match _maxSupply_ unless mintable or claimable, must be zero for claimable)
*   _reserveSupply_ is the reserve currency supply (in QNT) (must match _maxSupply_)
*   _maxSupply_ is the maximum currency supply (in QNT)
*   _issuanceHeight_ is the blockchain height at which a reservable currency is issued if the reserve minimum is met
*   _minReservePerUnitNQT_ is the minimum currency reserve (in NQT per QNT of _reserveSupply_) for issuance of a reservable currency
*   _minDifficulty_ is the minimum difficulty (minimum _1_) for a mintable currency
*   _maxDifficulty_ is the maximum difficulty (maximum _255_ and greater than _minDifficulty_) for a mintable currency
*   _ruleset_ is for future use, always set to zero
*   _algorithm_ is an algorithm code for a mintable currency: _2_ for SHA256, _3_ for SHA3, _5_ for Scrypt, _25_ for Keccak25
*   _decimals_ is the number of decimal places used by the currency (optional, default zero)

**Notes:** Reservable requires exchangeable and/or claimable, as does controllable; but mintable requires exchangeable. Claimable requires reservable, non-mintable and zero _initialSupply_.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the currency ID.

**Example:** Refer to [Issue Currency](API_Examples.md#issue-currency "The Blue0x API Examples") example.

### Publish Exchange Offer

Publish an exchange offer for an exchangeable currency. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _publishExchangeOffer_
*   _currency_ is the currency ID
*   _buyRateNQT_ is the offered buy rate (in NQT per QNT)
*   _sellRateNQT_ is the offered sell rate (in NQT per QNT)
*   _totalBuyLimit_ is the cumulative limit (in QNT) of currency buys
*   _totalSellLimit_ is the cumulative limit (in QNT) of currency sells
*   _initialBuySupply_ is the initial amount (in QNT) of currency offered to buy, cannot exceed _totalBuyLimit_
*   _initialSellSupply_ is the initial amount (in QNT) of currency offered to sell, cannot exceed _totalSellLimit_
*   _expirationHeight_ is the blockchain height for expiration of the offer

**Notes:** Each time currency is bought in response to an exchange request to sell currency (refer to [Currency Sell](#currency-buy-sell "The Blue0x API")), _totalBuyLimit_ is reduced and the supply of currency offered to sell increases by the amount bought. When _totalBuyLimit_ becomes zero, the buy offer is withdrawn. These same notes apply if _buy_ and _sell_ are interchanged. Only the most recent offer associated with an account is valid, even if an earlier offer by that account has not yet expired or reached its limits.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API"). The transaction ID is also the offer ID.

**Example:** Refer to [Publish Exchange Offer](API_Examples.md#publish-exchange-offer "The Blue0x API Examples") example.

### Search Currencies

Get currencies having a code that matches a given query in reverse relevance order.

**Request:**

*   _requestType_ is _searchCurrencies_
*   _query_ is a full text query on the currency field _code_ in the [standard Lucene syntax](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html#Overview)
*   _firstIndex_ is a zero-based index to the first currency to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last currency to retrieve (optional)
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _currencies_ (A) is an array of currency objects (refer to [Get Currency](#get-currency "The Blue0x API") for details)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Search Currencies](API_Examples.md#search-currencies "The Blue0x API Examples") example.

### Transfer Currency

Transfer currency to a given recipient. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _transferCurrency_
*   _recipient_ is the account ID of the transfer recipient
*   _currency_ is the currency ID
*   _units_ is the amount (in QNT) of the transfer

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Transfer Currency](API_Examples.md#transfer-currency "The Blue0x API Examples") example.

Networking Operations
------------------------

### Add Peer

Add a peer to the list of known peers and attempt to connect to it. Password protected like the [Debug Operations](#debug-operations "The Blue0x API"). POST only.

**Request:**

*   _requestType_ is _addPeer_
*   _peer_ is the IP address or domain name of the peer (plus optional port)

**Response:** refer to [Get Peer](#get-peer "The Blue0x API")

*   _isNewlyAdded_ is _true_ if the peer was not already known, omitted otherwise

**Example:** Refer to [Add Peer](API_Examples.md#add-peer "The Blue0x API Examples") example.

### Blacklist API Proxy Peer

Blacklist a remote node from the UI, so it won't be used when in roaming and light client modes. POST only.

**Request:**

*   _requestType_ is _blacklistAPIProxyPeer_
*   _peer_ is the IP address or domain name of the peer (plus optional port)
*   _adminPassword_ is a string with the admin password (optional)

**Response:**

*   _done_ (B) is _true_ if the peer is blacklisted
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Blacklist API Proxy Peer](API_Examples.md#blacklist-api-proxy-peer "The Blue0x API Examples") example.

### Blacklist Peer

Blacklist a peer for the default blacklisting period. Password protected like the [Debug Operations](#debug-operations "The Blue0x API"). POST only.

**Request:**

*   _requestType_ is _blacklistPeer_
*   _peer_ is the IP address or domain name of the peer (plus optional port)

**Response:**

*   _done_ (B) is _true_ if the peer is blacklisted
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Blacklist Peer](API_Examples.md#blacklist-peer "The Blue0x API Examples") example.

### Get Inbound Peers

Get all peers that have sent a request to this peer in the last 30 minutes.

**Request:**

*   _requestType_ is _getInboundPeers_
*   _includePeerInfo_ is _true_ to include peer information, otherwise include only the address (optional)

**Response:**

*   _peers_ (A) is an array of peer addresses or peer objects (refer to [Get Peer](#get-peer "The Blue0x API") for details) depending on _includePeerInfo_
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Inbound Peers](API_Examples.md#get-inbound-peers "The Blue0x API Examples") example.

### Get My Info

Get hostname and address of the requesting node.

**Request:**

*   _requestType_ is _getMyInfo_

**Response:**

*   _host_ (S) is the node hostname
*   _address_ (S) is the node address
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get My Info](API_Examples.md#get-my-info "The Blue0x API Examples") example.

### Get Peer

Get information about a given peer.

**Request:**

*   _requestType_ is _getPeer_
*   _peer_ is the IP address or domain name of the peer (plus optional port)

**Response:**

*   _hallmark_ (S) is the hex string of the peer's hallmark, if it is defined
*   _downloadedVolume_ (N) is the number of bytes downloaded by the peer
*   _address_ (S) the IP address or DNS name of the peer
*   _weight_ (N) is the peer's weight value
*   _uploadedVolume_ (N) is the number of bytes uploaded by the peer
*   _version_ (S) is the version of the software running on the peer
*   _platform_ (S) is a string representing the peer's platform
*   _lastUpdated_ (N) is the timestamp (in seconds since the genesis block) of the last peer status update
*   _blacklisted_ (B) is _true_ if the peer is blacklisted
*   _services_ (A) is an array of strings with the services the node provides
*   _blacklistingCause_ (S) is the cause of blacklisting (if _blacklisted_ is _true_)
*   _announcedAddress_ (S) is the name that the peer announced to the network (could be a DNS name, IP address, or any other string)
*   _application_ (S) is the name of the software application, typically _NRS_
*   _state_ (N) defines the state of the peer: 0 for NON\-cONNECTED, 1 for CONNECTED, or 2 for DISCONNECTED
*   _shareAddress_ (B) is _true_ if the address is allowed to be shared with other peers
*   _inbound_ (B) is _true_ if the peer has made a request to this node
*   _inboundWebSocket_ (B) is _true_ if an inbound websocket has been established from this node
*   _outboundWebSocket_ (B) is _true_ if an outbound websocket has been established to this node
*   _lastConnectAttempt_ (B) is the timestamp (in seconds since the genesis block) of the last connection attempt to the peer
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Peer](API_Examples.md#get-peer "The Blue0x API Examples") example.

### Get Peers

Get a list of peer IP addresses.

**Request:**

*   _requestType_ is _getPeers_
*   _active_ is _true_ for active (not NON\-cONNECTED) peers only (optional, if _true_ overrides _state_)
*   _state_ is the state of the peers, one of _NON\-cONNECTED_, -cONNECTED_, or -dISCONNECTED_ (optional)
*   _includePeerInfo_ is _true_ to include peer detail as in [Get Peer](#get-peer "The Blue0x API")
*   _service_ to filter on a specific service

**Note:** If neither _active_ nor _state_ is specified, all known peers are retrieved.

**Response:**

*   _peers_ (A) is an array of peer addresses
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Peers](API_Examples.md#get-peers "The Blue0x API Examples") example.

### Set API Proxy Peer

Set the remote node to use when in roaming and light client modes. POST only.

**Request:**

*   _requestType_ is _setAPIProxyPeer_
*   _peer_ is the IP address or domain name of the peer (plus optional port)
*   _adminPassword_ is a string with the admin password (optional)

**Response:**

*   _downloadedVolume_ (N) is the number of bytes downloaded by the peer
*   _address_ (S) the IP address or DNS name of the peer
*   _weight_ (N) is the peer's weight value
*   _uploadedVolume_ (N) is the number of bytes uploaded by the peer
*   _version_ (S) is the version of the software running on the peer
*   _platform_ (S) is a string representing the peer's platform
*   _blockchainState_ (S) is a string describing the state of the blockchain in the peer
*   _lastUpdated_ (N) is the timestamp (in seconds since the genesis block) of the last peer status update
*   _blacklisted_ (B) is _true_ if the peer is blacklisted
*   _services_ (A) is an array of strings with the services the node provides
*   _apiPort_ (N) is the API access port of the peer
*   _apiSSLPort_ (N) is the SSL API access port of the peer
*   _blacklistingCause_ (S) is the cause of blacklisting (if _blacklisted_ is _true_)
*   _announcedAddress_ (S) is the name that the peer announced to the network (could be a DNS name, IP address, or any other string)
*   _application_ (S) is the name of the software application, typically _NRS_
*   _state_ (N) defines the state of the peer: 0 for NON-CONNECTED, 1 for CONNECTED, or 2 for DISCONNECTED
*   _shareAddress_ (B) is _true_ if the address is allowed to be shared with other peers
*   _inbound_ (B) is _true_ if the peer has made a request to this node
*   _inboundWebSocket_ (B) is _true_ if an inbound websocket has been established from this node
*   _outboundWebSocket_ (B) is _true_ if an outbound websocket has been established to this node
*   _lastConnectAttempt_ (B) is the timestamp (in seconds since the genesis block) of the last connection attempt to the peer
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Set API Proxy Peer](API_Examples.md#set-api-proxy-peer "The Blue0x API Examples") example.

Phasing Operations
---------------------

### Approve Transaction

Approve (vote for) a phased transaction. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _approveTransaction_
*   _transactionFullHash_ is the full hash of the transaction to be approved (may be used up to 10 times per API request)
*   _revealedSecret_ is the secret phrase (required only for _phasingVotingModel 5_, refer to [Create Phasing Poll](#create-phasing-poll "The Blue0x API"))
*   _revealedSecretIsText_ is a way of specifying whether revealedSecret is text or binary.

**Note:** This transaction will be accepted in the blockchain only if all phased transactions it is voting for are already in it.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Approve Transaction](API_Examples.md#approve-transaction "The Blue0x API Examples") example.

### Create Phasing Poll

Create a phased transaction with conditional deferred execution based on the result of a vote, on a list of linked transactions or on the revelation of a secret; or simply with unconditional deferred execution. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is any type from the [Create Transaction](#create-transaction "The Blue0x API") list which is phasing-safe, indicated with italics such as -send Money_
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
*   _phasingHashedSecretAlgorithm_ is the hash function used: _2_ for SHA256, _6_ for RIPEMD160 and _62_ for SHA256 followed by RIPEMD160, according to [Get Constants](#get-constants "The Blue0x API") (required for a _phasingHashedSecret_)

**Note:** When a balance affects the poll result, the result depends only on the balance (including pending outgoing phased transfers) computed just prior to the finish height.

**Response:** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

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

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for details.

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

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for details.

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

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for details.

**Example:** Refer to [Get Currency Phased Transactions](API_Examples.md#get-currency-phased-transactions "The Blue0x API Examples") example.

### Get Linked Phased Transactions

Gets the phased transactions with by-transaction voting model for a given linkedFullHash, regardless of their phasing status (pending, approved or rejected). Since the corresponding table is trimmed after finish height however, the result will not include those transactions that finished before the last trimming height.

**Request:**

*   _requestType_ is _getLinkedPhasedTransactions_
*   -linkedFullHash_ is the full hash of the transaction transaction
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
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
*   _votingModel_ (N) is the voting model (refer to [Create Transaction Request](#create-transaction-request "The Blue0x API"))
*   _quorum_ (S) is the minimum number of votes needed to approve the poll
*   _transactionFullHash_ (S) is the full hash of the phasing poll transaction
*   _finished_ (B) is _true_ if the poll is finished, _false_ otherwise (omitted if _finished_ is _false_)
*   _result_ (S) is the sum of the _result_ of each account that approved (voted for) the transaction; an account's _result_ is _1_ if the voting model is _0_, _4_ or _5_; it is the NQT, asset QNT or currency QNT balance of the account if the voting model is _1_, _2_ or _3_ respectively; however, the _result_ is _0_ if _minBalance_ is not met
*   _approved_ (B) is _true_ if the poll was approved, _false_ otherwise
*   _minBalance_ (S) is the required minimum balance of voting accounts to be eligible to vote
*   _minBalanceModel_ (N) is the minimum balance model (refer to [Create Transaction Request](#create-transaction-request "The Blue0x API"))
*   _hashedSecret_ (S) is the hash of a secret that must be included in each approval (vote) transaction for the approval to be accepted (refer to [Create Transaction Request](#create-transaction-request "The Blue0x API"))
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

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for details.

**Example:** Refer to [Get Voter Phased Transactions](API_Examples.md#get-voter-phased-transactions "The Blue0x API Examples") example.

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
*   _includeCounts_ is _true_ if the fields beginning with _numberOf..._ are to be included (optional); password protected like the [Debug Operations](#debug-operations "The Blue0x API") if _true_.

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
*   _stage_ is the stage of the shuffling (See [Get Constants](#get-constants "The Blue0x API") for type definitions) (optional)
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
    *   _participantState_ (N) is the state for the participant (For more info, see shufflingParticipantStates array in [Get Constants](#get-constants "The Blue0x API"))
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
*   _holdingType_ (N) is the holding type (See [Get Constants](#get-constants "The Blue0x API") for type definitions)
*   _stage_ (N) is the current stage of the shuffling (See [Get Constants](#get-constants "The Blue0x API") for type definitions)
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
    *   _state_ (N) is the state for the participant (For more info, see shufflingParticipantStates array in [Get Constants](#get-constants "The Blue0x API"))
    *   _nextAccount_ (S) is the account ID of the next account in turn
    *   _nextAccountRS_ (S) is the account Reed Solomon address of the next account in turn
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Shuffling Participants](API_Examples.md#get-shuffling-participants "The Blue0x API Examples") example.

### Shuffling Cancel

Cancels a shuffling

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingCancel_
*   _shuffling_ is the shuffling ID
*   _shufflingStateHash_ is the state hash of the shuffling
*   _cancellingAccount_ is the account ID (optional)

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

### Shuffling Create

Creates a new shuffling.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingCreate_
*   _holding_ is the holding id (optional if holdingType is 0)
*   _holdingType_ is the holding type (See [Get Constants](#get-constants "The Blue0x API") for type definitions) (optional)
*   _amount_ is the amount of the holding to shuffle
*   _participantCount_ is the number of participants
*   _registrationPeriod_ is the number of blocks the participants have to register until the shuffle is cancelled

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Create](API_Examples.md#shuffling-create "The Blue0x API Examples") example.

### Shuffling Process

Manually process the shuffling for a specific participant. Note that the shuffling must be in processing stage and the _secretPhrase_ must match the current shuffling assignee.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingProcess_
*   _shuffling_ is the shuffling ID
*   _recipientSecretPhrase_ is the secret phrase of the recipient account (optional if _recipientPublicKey_ is provided)
*   _recipientPublicKey_ is the public key of the recipient account (optional if _recipientSecretPhrase_ is provided)

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Process](API_Examples.md#shuffling-process "The Blue0x API Examples") example.

### Shuffling Register

Registers a new participant to an existing shuffling. The shuffling must be in stage registration.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

*   _requestType_ is _shufflingRegister_
*   _shufflingFullHash_ is the full hash of the shuffling to register

**Response** Refer to [Create Transaction Response](index.md#create-transaction-response "The Blue0x API").

**Example:** Refer to [Shuffling Register](API_Examples.md#shuffling-register "The Blue0x API Examples") example.

### Shuffling Verify

Sends a verification that an account's recipient public key is found within a shuffling.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

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

Tagged Data Operations
-------------------------

Tagged data are similar to [prunable](#prunable-data "The Blue0x API") plain messages without a recipient, but with additional searchable metadata fields.

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

Token Operations
-------------------

### Decode File Token

Validate a file token without requiring the transmission of a secret passphrase. POST only.

**Request:**

*   _requestType_ is _decodeFileToken_
*   _file_ is the path to the file that was signed
*   _token_ is the token of the _file_, as generated by [Generate File Token](#generate-file-token "The Blue0x API")

**Response:**

*   _account_ (S) is the account number that generated the token
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _timestamp_ (N) is the time (in seconds since the genesis block) that the token was generated
*   _valid_ (B) is _true_ if token is valid, _false_ otherwise
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Since _token_ contains the token generator's public key and digital signature, _file_ can be validated as signed by the owner of the public key, and the public key determines the account ID.

**Example:** Refer to [Decode File Token](API_Examples.md#decode-file-token "The Blue0x API Examples") example.

### Decode Token

Validate a token without requiring the transmission of a secret passphrase.

**Request:**

*   _requestType_ is _decodeToken_
*   _website_ is the signed text, typically an authorized URL
*   _token_ is the token generated by [Generate Token](#generate-token "The Blue0x API")

**Response:**

*   _account_ (S) is the account number that generated the token
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _timestamp_ (N) is the time (in seconds since the genesis block) that the token was created
*   _valid_ (B) is _true_ if token is valid, _false_ otherwise
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Since _token_ contains the token generator's public key and digital signature, _website_ can be validated as authorized by the owner of the public key, and the public key determines the account ID.

**Example:** Refer to [Decode Token](API_Examples.md#decode-token "The Blue0x API Examples") example.

### Generate File Token

Generate a file token. POST only.

**Request:**

*   _requestType_ is _generateFileToken_
*   _secretPhrase_ is the passphrase of the account generating the token
*   _file_ is the path to the file to be signed

**Response:**

*   _token_ (S) is a 160 character string representing the 100-byte token which consists of a 32-byte public key, a 4-byte timestamp, and a 64-byte digital signature
*   _account_ (S) is the account number corresponding to _secretPhrase_
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _timestamp_ (N) is the time (in seconds since the genesis block) that the token was generated
*   _valid_ (B) is true if token is valid, false otherwise
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Since _token_ contains the token generator's public key and digital signature, the _file_ can be validated as digitally signed by the owner of the public key using [Decode File Token](#decode-file-token "The Blue0x API").

**Example:** Refer to [Generate File Token](API_Examples.md#generate-file-token "The Blue0x API Examples") example.

### Generate Token

Generate a token. POST only.

**Request:**

*   _requestType_ is _generateToken_
*   _secretPhrase_ is the passphrase of the account generating the token
*   _website_ is a web site URL for which authorization should be granted, or general text to be digitally signed

**Note:** _website_ is typically a URL (with the leading https:// unnecessary) that an account owner signs with his _secretPhrase_ (private key) to bind the account to the URL, but _website_ can be any text that the owner wishes to sign.

**Response:**

*   _token_ (S) is a 160 character string representing the 100-byte token which consists of a 32-byte public key, a 4-byte timestamp, and a 64-byte signature
*   _account_ (S) is the account number corresponding to _secretPhrase_
*   _accountRS_ (S) is the Reed-Solomon address of the account
*   _timestamp_ (N) is the time (in seconds since the genesis block) that the token was generated
*   _valid_ (B) is _true_ if token is valid, _false_ otherwise
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** Since _token_ contains the token generator's public key and signature, the _website_ can be validated as authorized by the owner of the public key using [Decode Token](#decode-token "The Blue0x API").

**Example:** Refer to [Generate Token](API_Examples.md#generate-token "The Blue0x API Examples") example.

Transaction Operations
-------------------------

### Broadcast Transaction

Broadcast a transaction to the network. POST only.

**Request:**

*   _requestType_ is _broadcastTransaction_
*   _transactionBytes_ is the bytecode of a signed transaction (optional)
*   _transactionJSON_ is the transaction object (optional if _transactionBytes_ provided)
*   _prunableAttachmentJSON_ is the attachment object embedded in _transactionJSON_ containing a prunable message (required if _transactionJSON_ not provided because _transactionBytes_ never includes prunable data)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _fullHash_ (S) is the full hash of the signed transaction
*   _transaction_ (S) is the transaction ID

**Example:** Refer to [Broadcast Transaction](API_Examples.md#broadcast-transaction "The Blue0x API Examples") example.

### Calculate Full Hash

Calculate the full hash of a transaction.

**Request:**

*   _requestType_ is _calculateFullHash_
*   _unsignedTransactionJSON_ is the unsigned transaction JSON object (optional)
*   _unsignedTransactionBytes_ are the unsigned bytes of a transaction (optional if _unsignedTransactionJSON_ is provided)
*   _signatureHash_ is a SHA-256 hash of the transaction signature

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _fullHash_ (S) is the full hash of the signed transaction

**Example:** Refer to [Calculate Full Hash](API_Examples.md#calculate-full_Hash "The Blue0x API Examples") example.

### Get Expected Transactions

Returns the non-phased unconfirmed transactions expected to be included in the next block (only), plus the phased transactions scheduled to finish in that block (whether approved or not).

**Request:**

*   _requestType_ is _getExpectedTransactions_
*   _account_ is one account ID (optional)
*   _account_ is one account ID (optional)

⋮

*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _expectedTransactions_ (A) is an array of expected transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Expected Transactions](API_Examples.md#get-expected-transactions "The Blue0x API Examples") example.

### Get Referencing Transactions

Gets the transactions referencing a given transaction id.

**Request:**

*   _requestType_ is _getReferencingTransactions_
*   _transaction_ is one transaction ID
*   _firstIndex_ is a zero-based index to the first block ID to retrieve (optional)
*   _lastIndex_ is a zero-based index to the last block ID to retrieve (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of transactions (refer to [Get Transaction](#get-transaction "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Referencing Transactions](API_Examples.md#get-referencing-transactions "The Blue0x API Examples") example.

### Get Transaction

Get a transaction object given a transaction ID.

**Request:**

*   _requestType_ is _getTransaction_
*   _transaction_ is the transaction ID (optional)
*   _fullHash_ is the full hash of the transaction (optional if transaction ID is provided)
*   _includePhasingResult_ is _true_ to retrieve execution status of each phased transaction (optional)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _sender_ (S) is the account ID of the sender
*   _senderRS_ (S) is the Reed-Solomon address of the sender
*   _feeNQT_ (S) is the fee (in NQT) of the transaction
*   _amountNQT_ (S) is the amount (in NQT) of the transaction
*   _transactionIndex_ (N) is a zero-based index giving the order of the transaction in its block
*   _timestamp_ (N) is the time (in seconds since the genesis block) of the transaction
*   _referencedTransactionFullHash_ (S) is the full hash of a transaction referenced by this one, omitted if no previous transaction is referenced
*   _confirmations_ (N) is the number of transaction confirmations
*   _subtype_ (N) is the transaction subtype (refer to [Get Constants](#get-constants "The Blue0x API") for a current list of subtypes)
*   _block_ (S) is the ID of the block containing the transaction
*   _blockTimestamp_ (N) is the timestamp (in seconds since the genesis block) of the block
*   _height_ (N) is the height of the block in the blockchain
*   _senderPublicKey_ (S) is the public key of the sending account for the transaction
*   _type_ (N) is the transaction type (refer to [Get Constants](#get-constants "The Blue0x API") for a current list of types)
*   _deadline_ (N) is the deadline (in minutes) for the transaction to be confirmed
*   _signature_ (S) is the digital signature of the transaction
*   _recipient_ (S) is the account number of the recipient, if applicable
*   _recipientRS_ (S) is the Reed-Solomon address of the recipient, if applicable
*   _fullHash_ (S) is the full hash of the signed transaction
*   _signatureHash_ (S) is a SHA-256 hash of the transaction signature
*   _approved_ (B) is a boolean indicating if the transaction is approved (only included when _includePhasingResult_ is true and the transaction is phased)
*   _result_ (S) is a string containing the result of the transaction (only included when _includePhasingResult_ is true and the transaction is phased)
*   _executionHeight_ (N) is the height the transaction was executed (only included when _includePhasingResult_ is true and the transaction is phased)
*   _transaction_ (S) is the transaction ID
*   _version_ (N) is the transaction version number
*   _phased_ (B) is _true_ if the transaction is phased, _false_ otherwise
*   _ecBlockId_ (N) is the economic clustering block ID
*   _ecBlockHeight_ (N) is the economic clustering block height
*   _attachment_ (O) is an object containing any additional data needed for the transaction, if applicable
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** The _block_, _blockTimestamp_ and _confirmations_ fields are omitted for unconfirmed transactions. Double-spending transactions are not retrieved.

**Example:** Refer to [Get Transaction](API_Examples.md#get-transaction "The Blue0x API Examples") example.

### Get Transaction Bytes

Get the bytecode of a transaction.

**Request:**

*   _requestType_ is _getTransactionBytes_
*   _transaction_ is a transaction ID
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _confirmations_ (N) is the number of transaction confirmations
*   _transactionBytes_ (S) are the bytes contained in the transaction
*   _unsignedTransactionBytes_ (S) are the unsigned bytes contained in the transaction
*   _prunableAttachmentJSON_ (O) is the prunable attachment JSON object, if applicable, because _transactionBytes_ never includes prunable data
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Transaction Bytes](API_Examples.md#get-transaction-bytes "The Blue0x API Examples") example.

### Parse Transaction

Get a transaction object given a (signed or unsigned) transaction bytecode, or re-parse a transaction object. Verify the signature.

**Request:**

*   _requestType_ is _parseTransaction_
*   _transactionBytes_ is the signed or unsigned bytecode of the transaction (optional)
*   _transactionJSON_ is the transaction object (optional if transactionBytes is included)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for additional fields.

*   _verify_ (B) is _true_ if the signature is verified, _false_ otherwise

**Example:** Refer to [Parse Transaction](API_Examples.md#parse-transaction "The Blue0x API Examples") example.

### Retrieve Pruned Transaction

Force retrieval of the prunable data for a given transaction, even if past the configured nxt.maxPrunableLifetime.

**Request:**

*   _requestType_ is _retrievePrunedTransaction_
*   _transaction_ is transaction ID

**Response:** Refer to [Get Transaction](#get-transaction "The Blue0x API") for fields.

**Example:** Refer to [Retrieve Pruned Transaction](API_Examples.md#retrieve-pruned-transaction "The Blue0x API Examples") example.

### Send Transaction

It broadcasts a transaction to the network without validating it, without re-broadcasting it and without adding it locally as unconfirmed transaction. Specially intended for roaming or light clients to send transactions to remote peers. POST only.

**Request:**

*   _requestType_ is _sendTransaction_
*   _transactionBytes_ is the bytecode of a signed transaction (optional)
*   _transactionJSON_ is the transaction object (optional if _transactionBytes_ provided)
*   _prunableAttachmentJSON_ is the attachment object embedded in _transactionJSON_ containing a prunable message (required if _transactionJSON_ not provided because _transactionBytes_ never includes prunable data)
*   _adminPassword_ is a string with the admin password (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   _fullHash_ (S) is the full hash of the signed transaction
*   _transaction_ (S) is the transaction ID

**Example:** Refer to [Send Transaction](API_Examples.md#send-transaction "The Blue0x API Examples") example.

### Sign Transaction

Calculates the full hash, signature hash, and transaction ID of an unsigned transaction.

**Request:**

*   _requestType_ is _signTransaction_
*   _unsignedTransactionJSON_ is the _transactionJSON_ field of the transaction, without a signature subfield
*   _unsignedTransactionBytes_ is the _unsignedTransactionBytes_ field of the transaction (optional, if _unsignedTransactionJSON_ provided)
*   _prunableAttachmentJSON_ is a prunable attachment JSON object, if applicable, because _unsignedTransactionBytes_ never includes prunable data (optional if the transaction has already been broadcast and the prunable message can still be retrieved from the database)
*   _secretPhrase_ is the secret passphrase of the signing account
*   _validate_ is _false_ to skip validation of the transaction bytes being signed (useful on nodes where the full blockchain is not downloaded)
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _signatureHash_ (S) is a SHA-256 hash of the transaction signature, used in escrow transactions
*   _verify_ (B) is _true_ the signature is verified, _false_ if not
*   _transactionJSON_ (O) is the signed transaction JSON object
*   _transactionBytes_ (S) are the signed transaction bytes
*   _fullHash_ (S) is the full hash of the signed transaction
*   _prunableAttachmentJSON_ (O) is the prunable attachment JSON object, if applicable, because _transactionBytes_ never includes prunable data
*   _transaction_ (S) is the transaction ID, derived from the _fullHash_
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Sign Transaction](API_Examples.md#sign-transaction "The Blue0x API Examples") example.

Voting System Operations
---------------------------

### Cast Vote

Cast a vote on a poll. POST only.

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

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

**Request:** Refer to [Create Transaction Request](#create-transaction-request "The Blue0x API") for common parameters.

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

⋮

**Notes:** Up to 100 options (answers) can be specified, but there is an extra fee for each option beyond 20. Unlike the API, the [NRS client](https://nxtwiki.org/wiki/Nxt_client_interface "Nxt client interface") treats a vote of _0_ as a nonvote not contributing to the number answers (options) chosen. The NRS client uses checkboxes for selecting answers when _minRangeValue_ = 0 and _maxRangeValue_ = 1; otherwise sliding controls are used to select answers from the allowed range.

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

Calculates the hash of a secret for use in [phased transactions](#create-phasing-poll "The Blue0x API") with voting model 5 (_Vote By Secret_).

**Request:**

*   _requestType_ is _hash_
*   _hashAlgorithm_ is the hash function used: _2_ for SHA256, _3_ for SHA3, _5_ for SCRYPT, _6_ for RIPEMD160, _25_ for Keccack25 and _62_ for SHA256 followed by RIPEMD160, according to [Get Constants](#get-constants "The Blue0x API")
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

**Note:** Java does not support unsigned integers, so any unsigned ID (such as a block ID) visible in the [NRS client](https://nxtwiki.org/wiki/Nxt_client_interface "Nxt client interface") is represented internally as a signed integer.

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

Debug Operations
-------------------

All debug utilities require an _adminPassword_ request parameter. See [Admin Password](#admin-password "The Blue0x API") for more info.

### Clear Unconfirmed Transactions

Empties the unconfirmed transaction pool. POST only.

**Request:**

*   _requestType_ is _clearUnconfirmedTransactions_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Clear Unconfirmed Transactions](API_Examples.md#clear-unconfirmed-transactions "The Blue0x API Examples") example.

### Dump Peers

Get all active peers, optionally of a certain version or a minimum weight.

**Request:**

*   _requestType_ is _dumpPeers_
*   _version_ is a version filter such as _1.5.11_ (optional)
*   _weight_ is a minimum weight filter such as 1000 (optional)
*   _connect_ is _true_ to force a connection attempt to each known peer first (optional); password protected like the [Debug Operations](#debug-operations "The Blue0x API") if _true_

**Response:**

*   _peers_ (S) is a string of peer IP addresses or DNS names, separated by semicolons
*   _count_ (N) is the number of peers in the _peers_ string.
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Dump Peers](API_Examples.md#dump-peers "The Blue0x API Examples") example.

### Full Reset

Deletes the entire blockchain. POST only.

**Request:**

*   _requestType_ is _fullReset_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Note:** After successful completion of the reset, a new blockchain will automatically begin downloading.

**Example:** Refer to [Full Reset](API_Examples.md#Full-reset "The Blue0x API Examples") example.

### Get All Broadcasted Transactions

Get unconfirmed transactions broadcasted from this node but not yet received back from a peer, if transaction rebroadcasting is enabled.

**Request:**

*   _requestType_ is -getAllBroadcastedTransactions_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of broadcasted unconfirmed transactions not yet received back from a peer (S)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Broadcasted Transactions](API_Examples.md#get-all-broadcasted-transactions "The Blue0x API Examples") example.

### Get All Waiting Transactions

Get unconfirmed transactions temporarily kept in memory during transaction processing.

**Request:**

*   _requestType_ is _getAllWaitingTransactions_
*   _requireBlock_ is the block ID of a block that must be present in the blockchain during execution (optional)
*   _requireLastBlock_ is the block ID of a block that must be last in the blockchain during execution (optional)

**Response:**

*   _transactions_ (A) is an array of unconfirmed transactions temporarily kept in memory (S)
*   _lastBlock_ (S) is the last block ID on the blockchain (applies if _requireBlock_ is provided but not _requireLastBlock_)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get All Waiting Transactions](API_Examples.md#get-all_Waiting-transactions "The Blue0x API Examples") example.

### Get Log

Get up to 100 of the most recent log messages from a memory buffer.

**Request:**

*   _requestType_ is _getLog_
*   _count_ is the number of messages to return (optional, default 100)

**Response:**

*   _messages_ (A) is an array of log messages (S)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Get Log](API_Examples.md#get-log "The Blue0x API Examples") example.

### Get Stack Traces

Get the stack traces of the currently running threads in reverse _id_ order.

**Request:**

*   _requestType_ is _getStackTraces_
*   _depth_ is the maximum trace depth to retrieve (optional)

**Response:**

*   _requestProcessingTime_ (N) is the API request processing time (in millisec)
*   -locks_ (A) is an array of lock objects
*   _threads_ (A) is an array of thread objects with the following fields:
    *   _trace_ (A) is an array of traces (S)
    *   _name_ (S) is the thread name
    *   _id_ (N) is the thread ID
    *   _state_ (S) is the thread state, one of _WAITING_, -tIMED\_WAITING_ and -rUNNABLE_
    *   -locks_ (A) is an array of lock objects with the following fields, if not empty:
        *   _trace_ (S)
        *   _depth_ (N)
        *   _name_ (S)
        *   _hash_ (N)

**Example:** Refer to [Get Stack Traces](API_Examples.md#get-stack-traces "The Blue0x API Examples") example.

### Lucene Reindex

Forces a rebuild of the Lucene search index. POST only.

**Request:**

*   _requestType_ is -luceneReindex_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Lucene Reindex](API_Examples.md#lucene-reindex "The Blue0x API Examples") example.

### Pop Off

Removes specified number of blocks (and associated transactions) from the top of the blockchain. POST only.

**Request:**

*   _requestType_ is _popOff_
*   _numBlocks_ is the number of blocks to pop off the blockchain (optional)
*   _height_ is the new height of the blockchain after popping (optional if _numBlocks_ provided)

**Note:** If table trimming is enabled (default), at most 1440 blocks can be popped off without triggering a full rescan.

**Response:**

*   _blocks_ (A) is an array of the blocks popped off (refer to [Get Block](#get-block "The Blue0x API") for details)
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Pop Off](API_Examples.md#pop-off "The Blue0x API Examples") example.

### Rebroadcast Unconfirmed Transactions

Rebroadcast transactions in the unconfirmed pool to peers, until received back or found in the blockchain. Rebroadcasting can be disabled by setting the _nxt.enableTransactionRebroadcasting_ property to _false_. POST only.

**Request:**

*   _requestType_ is -rebroadcastUnconfirmedTransactions_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Rebroadcast Unconfirmed Transactions](API_Examples.md#rebroadcast-unconfirmed-transactions "The Blue0x API Examples") example.

### Requeue Unconfirmed Transactions

Requeue unconfirmed transactions. POST only.

**Request:**

*   _requestType_ is _requeueUnconfirmedTransactions_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Requeue Unconfirmed Transactions](API_Examples.md#requeue-unconfirmed-transactions "The Blue0x API Examples") example.

### Retrieve Pruned Data

Initiates a task of requesting and restoring missing prunable data. POST only.

**Request:**

*   _requestType_ is _retrievePrunedData_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _numberOfPrunedData_ (N) is the number of pruned data available pruned data transactions
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Retrieve Pruned Data](API_Examples.md#retrieve-pruned-data "The Blue0x API Examples") example.

### Scan

Scans the top of the blockchain. POST only.

**Request:**

*   _requestType_ is _scan_
*   _numBlocks_ is the number of blocks to scan at the top of the blockchain (optional)
*   _height_ is the height above which blockchain is to be scanned (optional if _numBlocks_ provided)
*   _validate_ is _true_ if signatures are to be re-verified and blocks and transactions re-validated (optional)

**Note:** The derived object tables are rolled back and rebuilt by rescanning the existing blockchain. A request to rescan more than 1440 blocks when table trimming is enabled will do a full rescan starting from height 0. Rescan status is saved in the database, so that if a rescan is interrupted or fails it will resume on restart.

**Response:**

*   _scanTime_ (N) is the scan time
*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Scan](API_Examples.md#scan "The Blue0x API Examples") example.

### Set Logging

Sets the log level and optionally specifies communication events to be logged, without restarting the server. POST only.

**Request:**

*   _requestType_ is _setLogging_
*   -logLevel_ is one of -eRROR_, _WARN_, -iNFO_ or -dEBUG_ with each level in the list including all of the previous levels (optional, default is -iNFO_)
*   _communicationEvent_ is one of multiple communication (HTTP) events to be logged, from the list: -eXCEPTION_, _HTTP-eRROR_, _HTTP-OK_ (optional)
*   _communicationEvent_ is one of multiple communication events (optional)

⋮

**Note:** The initial log level is set by the _nxt.level_ logging property, currently -fINE_ (equivalent to -dEBUG_).

**Response:**

*   -loggingUpdated_ (B) is _true_ if the operation completed successfully

**Example:** Refer to [Set Logging](API_Examples.md#set-logging "The Blue0x API Examples") example.

### Shutdown

Shutdown the server. POST only.

  
**Request:**

*   _requestType_ is _shutdown_
*   _scan_ is _true_ to truncate the derived tables and schedule a full rescan with validation on the next start (optional)

**Response:**

*   _shutdown_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Shutdown](API_Examples.md#shutdown "The Blue0x API Examples") example.

### Trim Derived Tables

Trigger a derived tables trim, and a prunable tables pruning. POST only.

  
**Request:**

*   _requestType_ is _trimDerivedTables_

**Response:**

*   _done_ (B) is _true_ if the operation completed successfully
*   _requestProcessingTime_ (N) is the API request processing time (in millisec)

**Example:** Refer to [Trim Derived Tables](API_Examples.md#trim-derived-tables "The Blue0x API Examples") example.
