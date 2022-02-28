## **Setup your Blue0x Mac Desktop Wallet** ##

1. Install Homebrew (if you haven’t done it already)

  	- `/bin/bash -c “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)”`

2. Update (if you haven’t done it already)

  	- `brew update`

3. Install Java

  	- `brew install openjdk`

4. Symlink your Java installation

  	- `sudo ln -sfn $(brew --prefix)/opt/openjdk/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk`

## Download and Connect to the Blue0x Network

1. [Download](https://github.com/theBlue0x/desktop-wallet/releases/download/Blue0x-Desktop-Wallet-v1.12.2/Blue0x-Desktop-Wallet-v1.12.2.zip) the latest release from the official Blue0x GitHub repository.

	- Once the download has finished, extract the folder to a desired location.
  
2. Connect to the Blue0x Network.

	- From a terminal, navigate to the Blue0x Desktop Wallet folder. 
	
	- Type: `java -jar Blue0x.jar`
	
	You will now see your computer begin to sync with the Blue0x Network. When you see "Finished Blockchain Download" , you may proceed.
	
3. Create a new account.

	- From your favorite browser, navigate to [http://localhost:2022](http://localhost:2022).
  
	- You will see the prompt for Creating a New Account.  Make sure to save and securely store your new account passphrase.
	
	- Once account setup is completed, send us your address via [Discord](https://discord.gg/EbBWRSPW63). 
	
Welcome to Blue0x!
