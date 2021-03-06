## **Deploy a Public Blue0x Node** ##

**1. Sign up for a VPS (Virtual Private Server) with a provider like [DigitalOcean.com](https://m.do.co/c/97a921447f80).**
>These instructions assume you are using DigitalOcean, but other VPS providers are similar.  

[DigitalOcean $100 Credit for All New Users](https://m.do.co/c/97a921447f80)

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg)](https://www.digitalocean.com/?refcode=97a921447f80&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)


**2. Once signed up you will have a $100 credit for 60 days.**

>The $5 monthly plan covers 1 month of computing for 1 server with 1 GB RAM and 25 GB storage. This is sufficient, but not ideal. 

>The $10 monthly plan, with 2 GB RAM and 50 GB storage, is better suited to run the Blue0x software.


**3. Create your first Droplet.**

>Click "Create" and then "Droplet" from the dropdown menu.

>Select "Ubuntu 20.04(LTS) x64".

>Select the "Basic", "Regular Intel with SSD", and either the $5/mo or $10/mo option.

>Choose any datacenter that you would like.

>Enter a secure root password.  You will need this later.

>Choose a hostname if you would like or leave it as default.

>Click "Create Droplet".

>In a few moments, your new VPS will be ready to go.

**4. Connect to your new Droplet.**

>If you are on *Windows*, download [Putty.](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html )  Once installed, fill in the IP Address of your VPS into the 'Host Name' field. Confirm that the 'Port' is set to '22' and that the 'Connection type' is 'SSH'.  In the 'Connection' subheading, under 'SSH' ensure '2' is selected under 'SSH protocol version'. Under 'Data' subheading, enter 'root' as 'Auto-login username'.  You may then connect by clicking 'Open'.  Once started, you will then need to enter the password you specified in the last step.

>If you are on a *Mac*, you can use Terminal, which is built into MacOS.

>If you are on a *UNIX* system, you can use any shell to connect via ssh root@IP_ADDRESS_VPS.

>_NOTE: The following steps will set up your Blue0x node to run as 'root'. This is risky, since it can give full access to your VPS if it is compromised. It is advised to set up a separate user account to run your BLX node. Setting up a user account is outside the scope of this tutorial and should be researched further to run as a user other than 'root'._

**5. After logged in to your VPS, install Java.**

>Run the following commands in your terminal

>$ sudo apt update

>$ sudo apt install default-jdk

**6. Download the Blue0x Software.**
>$ git clone https://github.com/theBlue0x/node.git

**7. Compile and Run Blue0x.**
>$ cd node

>$ ./compile.sh

>$ ./run.sh 

>You should see a final message indicating that the node is running at "localhost:2022"

**8. Ensure Your Node is Available.**

>In your browser, open 'IP_ADDRESS_OF_VPS:2022/index.html'

>You should see the Blue0x Login Screen.

**9. Create A New Account and Register Your Node for Node Rewards.**

>Click New User.

>Be sure to save and securely store your new account passphrase.

>Once you are complete with setting up your account, stop your node by pressing CTRL+C.

>You will need to add your new account address to your nxt.properties file.

>$ nano conf/nxt.properties

>Add nxt.myPlatform=BLX-XXXX-XXXX-XXXX-XXXX where BLX-XXXX-XXXX-XXXX-XXXX is your new account address.

>CTRL+X, ENTER to save the file.

>Restart your node as a background service by running ./run.sh --daemon.

>You can check to make sure your node is properly configured [here](https://blue0x.com/node.html).

Congratulations!  You are now running a public Blue0x Node!  Your rewards will arrive soon!
