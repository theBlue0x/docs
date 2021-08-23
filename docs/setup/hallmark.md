### **Creating a Hallmark for Your Node** ###

A hallmark is a "stamp of approval" for a Blue0x node and is intended for users running a [dedicated node on a VPS or dedicated server](vps.md).  By creating a digital signature based on your IP address and secret passphrase (private key), you are verifying that your account "owns" a node and is accountable for it.  This helps protect the network from attack, and increases the network's trust in your node.   Hallmarking has no impact on forging operations.  

To qualify for [Node Rewards](../node_rewards/index.md), you will need to hallmark your node.

**To Create a Hallmark:**

**1. Navigate to http://localhost:2022/test or https://IP_ADDRESS OR DOMAIN/test.**

>Under ALL, navigate to the markHost tab. You will need to complete fields:

>**Public IP:** The IP Address of your Blue0x node, or, if you are advertising a DNS name in your properties file for the setting of: nxt.myAddress= then use this DNS name here as well instead of the IP address. If using non-default port, the hallmark must also include that port, otherwise the hallmark is ignored.

>**Weight:** A weighting value for your node. (Example: 1)

>If you only operate one node, the value doesn't matter... but if you operate multiple nodes, you can use this value to set a relative "weight" for each node.  The network will "sum" all of the weights for your hallmarked node, and use the ratios of those weights to determine how much traffic each one can bear.  For example, if you have four nodes and want them all weighted equally, set equal values.  But if one node has much more memory and bandwidth allocated to it, you may weight it more highly relative to the other three.

>**Date:** Today's date.

>**Secret phrase:** Your account secret passphrase.

>Click "Submit".

>The server will generate a hallmark value for you, in the form of a long string of text.  The response from the server will look something like this:

>{

 >"hallmark": "57fb6f3a958e320bb49c4e81b4c2cf28b9f25d086c143b473beec228f79ff93c...",

 >"requestProcessingTime": 2

>}

**2. Edit the _nxt.properties_ file for your node:**

>If it is running, stop the client.

>$ nano conf/nxt.properties

>Add the text string from the previous step to nxt.myHallmark= 

>Example:
 nxt.myHallmark=57fb6f3a958e320bb49c4e81b4c2cf28b9f25d086c143b473beec228f79ff93c...

>Press CTRL+X and then Y to save the file.


**3. Restart Blue0x to activate the Hallmark.**

Congratulations, your Blue0x node is now Hallmarked!

If you wish, you may now enter your node in the Blue0x [Node Rewards](../node_rewards/index.md) program.