Networking Operations
------------------------

### Add Peer

Add a peer to the list of known peers and attempt to connect to it. Password protected like the [Debug Operations](debug.md "The Blue0x API"). POST only.

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