## **Configuring HTTPS on a Standalone Blue0x Installation** ##

_**Note:**This tutorial uses [Certbot](https://certbot.eff.org/) to issue certificates.
Follow the official documentation to [install Certbot](https://certbot.eff.org/instructions) before beginning this tutorial._

**1. Add the following lines to your _nxt.properties_ file.**

>nxt.apiSSL=true

>nxt.keyStorePath=keystore

>nxt.keyStorePassword='PutYourLongPasswordHere'

>nxt.keyStoreType=PKCS12

**2. Issue a Certificate.**

>$ sudo certbot certonly --standalone -d DOMAIN

>Replace DOMAIN with your domain name. You will be asked for your email during setup.  If everything is correct, you should have your certificate and private key under the Let's Encrypt directory. Usual locations are /etc/letsencrypt/live/DOMAIN and /usr/local/etc/letsencrypt/live/DOMAIN.

**3. Export the Certificate to a Java Keystore.**

>$ sudo openssl pkcs12 -export -in /etc/letsencrypt/live/DOMAIN/fullchain.pem -inkey /etc/letsencrypt/live/DOMAIN/privkey.pem -out KEYSTORE -name nrs -passout pass:KEYSTORE_PASS

> Replace DOMAIN with your domain name and replace KEYSTORE_PASS with the password you set in _nxt.properties_ under _nxt.keyStorePassword_.

**4. Configure Renewal of Certificate.**

>The Let's Encrypt certificates are issued with a short expiry date. The recommended approach to renewal is to automate it and the Certbot software provides support for automatic renewal. But Blue0x software doesn't use the Let's Encrypt certificate directly as we've seen in previous sections so we need to update the keystore with each renewal.

>$ sudo certbot certonly -d DOMAIN -n --standalone --deploy-hook "/PATH/TO/BLUE0X_INSTALLATION/pem.to.pkcs12.keystore.certbot.hook.sh" --force-renewal

>Replace DOMAIN with your domain name. This will force the renewal, execute the script, and also update the certbot configuration to use the script with every certificate renewal for that domain.  The script pem.to.pkcs12.keystore.certbot.hook.sh is included with the Blue0x software.  You may check the script [here](renewal_script.md).

**5. Allow Blue0x to Listen on a Privileged Port.**

>If you want your node to listen on a privileged port, like the standard HTTPS port 443, you need root privileges or an alternative mechanism. It is not recommend to run your node, or any nonsystem software, as root.  Authbind, a tool available in Ubuntu and other Debian derivates, allows the Blue0x software to listen to port 443 without running it as root.

>$ sudo apt install authbind

>$ sudo touch /etc/authbind/byport/443

>Authbind determines that a process can bind to that port if it has execute permissions to that file. Let's suppose you are running your node as the local user 'blue0x'. You will need to execute these commands:

>$ sudo chgrp blue0x /etc/authbind/byport/443

>$ sudo chmod g+x /etc/authbind/byport/443

>Now Authbind will provide a mechanism for any process run by that user to just bind to port 443. In order to use that mechanism you need to run your command inside a Authbind execution. You can use the --authbind modifier of run.sh for that purpose.

>$ ./run.sh --daemon --authbind

**6. Test Your Installation.**

>In your favorite browser, navigate to the domain name of your node and you should see the 'lock' icon next to the address field.  You should not have to specify port number as it is now served over SSL port 443.

Congratulations, your Blue0x node is now secured over HTTPS!



