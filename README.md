# SafeGames Masternode Setup Guide for Windows with a Ubuntu 16.04 VPS

This guide is for a single masternode, on a Ubuntu 16.04 64bit server (VPS) running headless and will be controlled from the wallet on your local computer (Control wallet). The wallet on the the VPS will be referred to as the Remote wallet.
You will need your server details for progressing through this guide.


## Requirements:

1. 5000 SGS
1. A main computer (Your everyday computer) – This will run the control wallet, hold your collateral 5000 SGS and can be turned on and off without affecting the masternode.
1. Masternode Server (VPS – The computer that will be on 24/7)
   1. The VPS needs a static IP-adress.
  
For security reasons, you’re are going to need a different IP for each masternode.
The basic reasoning for these requirements is that, you get to keep your SGS in your local wallet and host your masternode remotely, securely.

## Private Key 

Steps generate your own private key. 
1.  Download and install Safe windows wallet from [Releases](https://github.com/safegames/safegames/releases)
2.  Go to **Tools -> Click "Debug Console"** 
3.  Type the following command:

``masternode genkey ``

4. You now have your generated **Private Key**  (MasternodePrivKey)

## VPS installation
First you will need a VPS to continue on with this guide. If you do not have one get one from here [Vultr.](https://www.vultr.com/?ref=7433477)

Next step is to download the latest release for linux on the vps with the commands below.
1.	Go to your home directory:

``cd ~``

2.	From your home directory, download the latest version from the Safegames GitHub repository:

``wget https://github.com/safegames/safegames/releases/download/v1.0.0/safegames-1.0.0-x64-linux.tar.gz``

3.	Unzip and extract:

``tar -zxvf safegames-1.0.0.-x86_64-linux-gnu.tar.gz``

4.	Go to your Safegames 1.0.0 bin directory:

``cd ~/safegames-1.0.0/bin``

5.	Note: If this is the first time running the wallet in the VPS, you’ll need to attempt to start the wallet ./safegamesd this will place the config files in your ~/.safegames data directory

``./safegamesd ``

1.	press **CTRL+C** to exit / stop the wallet then continue to step 8

8) Now on the masternodes, find the Safegames data directory here.(Linux: ~/.safegames)

``cd ~/.safegames``

9) Open the safegames.conf by typing nano safegames.conf and make the config look like this:

``nano safegames.conf``

```
 rpcuser=INSERTRANDOMUSERNAME
 rpcpassword=INSERTRANDOMPASSWORD
 rpcallowip=127.0.0.1
 server=1
 daemon=1
 logtimestamps=1
 maxconnections=256
 masternode=1
 externalip=VPS-IP-ADDRESS
 masternodeprivkey=MASTERNODE_PRIVKEY
 ```

Make sure to replace **rpcuser** and **rpcpassword** with a random username and password.

Insert your VPS IP-Adress and Masternode Private key, you generated earlier.

10) to exit the editor press esc then **CTRL+X** and save the file by pressen **Y** for Yes.

## Start your masternode
11) Now, you need to finally start these things in this order
– Start the daemon client in the VPS. First go back to your installed wallet directory, cd ~/safegames-1.0.0/bin and then start the wallet using ./sagegamesd

``cd ~/safegames-1.0.0/bin ``
``./sagegamesd``


# Desktop wallet setup
**Note: The auto zSGS minter should be disabled during this setup to prevent autominting of your masternode collateral. BEFORE unlocking your wallet, you can disable autominting in the control wallet option menu.**

![Disable zSGS](https://github.com/safegames/documentation/blob/master/images/zSGS%20disable.gif)


1. Using the control wallet, enter the debug console (Tools > Debug console) and type the following command:

``masternode genkey ``

This will be the masternode’s privkey. We’ll use this late
2. Using the control wallet still, enter the following command:

``getaccountaddress MN1``

MN1 is just an example, you can choose whatever you like.

3. Send 5000 SGS to the address you generated before.
**Be 100% sure that you entered the address correctly. You can verify this when you paste the address into the “Pay To:” field, the label will autopopulate with the name you chose”, also make sure this is exactly 5000 SGS**

4. Still in the control wallet, enter the command into the console:

``masternode outputs`` 

This gets the proof of transaction of sending 5000 SGS

5. Still on the main computer, go into the Safegames data directory, by default in Windows it’ll be %Appdata%/safegames Linux~
Find masternode.conf and add the following line to it:

``Alias Address Privkey TxHash TxIndex``

* Alias: **MN1**
* Address: **VPS_IP:61555**
* Privkey: **Masternode Private Key**
* TxHash: **First value from Step 4**
* TxIndex:  **Second value from Step 4**

**This is a example of how your masternode.conf should look like. (This should all be on one line): 

``MN1 31.14.135.27:61555 sgsWPpkqbr7sr6Si4fdsfssjjapuFzAXwETCrpPJubnrmU6aKzh c8f4965ea57a68d0e6dd384324dfd28cfbe0c801015b973e7331db8ce018716999 1``

Save the masternode.conf file and restart the wallet.

From the Control wallet debug console type:

``startmasternode alias false <mymnalias>``

where <mymnalias> is the name of your masternode alias (without brackets)
The following should appear:
 ```
“overall” : “Successfully started 1 masternodes, failed to start 0, total 1”,
“detail” : [
{
“alias” : “<mymnalias>”,
“result” : “successful”,
“error” : “”
}
```
– Back in the VPS (remote wallet), start the masternode

``./safegames-cli startmasternode local false``

– A message “masternode successfully started” should appear

12)Use the following command to check status:

``./safegames-cli masternode status``

You should see something like:
```
{
“txhash” : “334545645643534534324238908f36ff4456454dfffff51311”,

“outputidx” : 0,

“netaddr” : “31.14.135.27:61555”,

“addr” : “S6fujc45645645445645R7TiCwexx1LA1”,

“status” : 4,

“message” : “Masternode successfully started”
}
```
You want to see **"Masternode started successfully and Status 4"**

## Congratulations! You have successfully created your masternode!
Now the masternode setup is complete, you are safe to remove “enablezeromint=0” from the safegames.conf file of the control wallet.

# Tearing down a Masternode
1) How do I stop running MN1 on my VPS hoster and delete MN1 from my ‘Safegames Core – Wallet’?
a) ./safegames-cli stop from the masternode to stop the wallet.
b) Then from your controller wallet PC, edit your masternode.conf, delete the MN1 masternode line entry.
c) Now restart the controller wallet.
d) your 5K will now be unlocked.
2) How do I get the 10k back that I’ve send to my MN1 address at the beginning of the MN1 setup?
You don’t need to “get that back” as it is already in your wallet.
Being in the different address is not an issue as that’s also your address.
3) Can I use this 10k normally on my wallet then gain, and sell it or stake it normally like before?
Yes!  

