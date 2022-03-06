## **Connect via Windows** ##

You will need the latest Java JDK installed on your computer and you will need to set your JAVA_HOME PATH.

1. To install the JDK software, do the following:
    
    -  Go to  [http://java.sun.com/javase/downloads/index.jsp](http://java.sun.com/javase/downloads/index.jsp).
        
    -  Select "Windows" ⇒ Download the "x64 Installer" (e.g., "jdk-17_windows-x64_bin.exe" - about 152MB) and click Download.

    -  Run the downloaded installer. Accept the defaults and follow the on screen instructions to complete the installation.
                
2.  Now we will set the PATH environment Variable:

    -  Right click on My Computer or This PC(in Windows 10+) and select the option Properties

    -  A new window with System properties will open. From the left sidebar, click on the option Advanced system settings.

    -  Then click on Environment Variables button on the bottom right corner.

    -  Add the location of the bin folder of the JDK installation (for example, C:\Program Files\Java\jdk-17\bin) to the PATH variable in User Variables.
    
3.  Setting the JAVA_HOME environment variable:
            
    -  Under System variable, click new to add a new variable with name JAVA_HOME or if the JAVA_HOME variable exists, select it and click on Edit to edit it.

    -  Now enter the path of JDK installation as value for it (without the bin folder, for example, C:\Program Files\Java\jdk-17)

    -  Click OK and then click on Apply changes.

## Download and Connect to the Blue0x Network

1. [Download](https://github.com/theBlue0x/desktop-wallet/releases/download/Blue0x-Desktop-Wallet-v1.12.2/Blue0x-Desktop-Wallet-v1.12.2.zip) the latest release from the official Blue0x GitHub repository.

	- Once the download has finished, extract the folder to a desired location.
  
2. Connect to the Blue0x Network.

	- Navigate to the Blue0x Desktop Wallet folder (the folder you just extracted) and while holding down the SHIFT key, right click and select *Open a command line from here*.
	
	- From this command line, type: `java -jar Blue0x.jar`
	
	- You will now see your computer begin to sync with the Blue0x Network. When you see "Finished Blockchain Download" , you may proceed.
	
3. Create a new account.

	-  From your favorite browser, navigate to [http://localhost:2022](http://localhost:2022).  
  
	- You will see the prompt for Creating a New Account.  Make sure to save and securely store your new account passphrase.
	
	- Once account setup is completed, send us your address via [Discord](https://discord.gg/EbBWRSPW63). 
	
	
Welcome to Blue0x!
