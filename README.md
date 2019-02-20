``Dank meme``

# Introduction
This guide is for a single masternode, on a Ubuntu 16.04 64bit server (VPS) running headless and will be controlled from the wallet on your local computer (Control wallet). The wallet on the the VPS will be referred to as the Remote wallet.
You will need your server details for progressing through this guide.
First the basic requirements:
1.	5000 SGS
2.	A main computer (Your everyday computer) – This will run the control wallet, hold your collateral 5000 SGS and can be turned on and off without affecting the masternode.
3.	Masternode Server (VPS – The computer that will be on 24/7)
4.	A unique IP address for your VPS / Remote wallet
(For security reasons, you’re are going to need a different IP for each masternode you plan to host)
The basic reasoning for these requirements is that, you get to keep your SGS in your local wallet and host your masternode remotely, securely.

# Configuration
** Note: The auto zSGS minter should be disabled during this setup to prevent autominting of your masternode collateral. BEFORE unlocking your wallet, you can disable autominting in the control wallet option menu.

![Disable zSGS](https://github.com/safegames/documentation/blob/master/images/zSGS%20disable.gif)


1) Using the control wallet, enter the debug console (Tools > Debug console) and type the following command:
masternode genkey (This will be the masternode’s privkey. We’ll use this later…)
2) Using the control wallet still, enter the following command:
getaccountaddress chooseAnyNameForYourMasternode
5.	3) Still in the control wallet, send 5000 SGS
6.	to the address you generated in step 2 (Be 100% sure that you entered the address correctly. You can verify this when you paste the address into the “Pay To:” field, the label will autopopulate with the name you chose”, also make sure this is exactly 5000 SGS; No less, no more.)
– Be absolutely 100% sure that this is copied correctly. And then check it again. We cannot help you, if you send 5000 SGS to an incorrect address.
4) Still in the control wallet, enter the command into the console:
masternode outputs (This gets the proof of transaction of sending 5000)
5) Still on the main computer, go into the Safegames data directory, by default in Windows it’ll be%Appdata%/safegamesor Linux~
Find masternode.conf and add the following line to it:
 <Name of Masternode(Use the name you entered earlier for simplicity)> <Unique IP address>:61555<The result of Step 1> <Result of Step 4> <The number after the long line in Step 4>

Example: MN1 31.14.135.27:61555 sgsWPpkqbr7sr6Si4fdsfssjjapuFzAXwETCrpPJubnrmU6aKzh c8f4965ea57a68d0e6dd384324dfd28cfbe0c801015b973e7331db8ce018716999 1
Substitute it with your own values and without the “<>”s

# VPS Remote wallet install
7) Install the latest version of the SGS wallet onto your masternode. 
The lastest version can be found here: https://github.com/safegames/safegames/releases/tag/v1.0.0

1.	Go to your home directory:

``cd ~``

2.	From your home directory, download the latest version from the Safegames GitHub repository:

``wget https://github.com/safegames/safegames/releases/download/v1.0.0/safegames-1.0.0-x64-linux.tar.gz``

The link above is for Ubuntu (or similar), make sure you choose the correct version of the core wallet if you are not using Ubuntu from: https://github.com/safegames/safegames/releases

3.	Unzip and extract:

``tar -zxvf safegames-1.0.0.-x86_64-linux-gnu.tar.gz``

4.	Go to your Safegames 1.0.0 bin directory:

``cd ~/safegames-1.0.0/bin``

5.	Note: If this is the first time running the wallet in the VPS, you’ll need to attempt to start the wallet ./safegamesd this will place the config files in your ~/.safegames data directory

``./safegamesd ``

1.	press CTRL+C to exit / stop the wallet then continue to step 8

8) Now on the masternodes, find the Safegames data directory here.(Linux: ~/.safegames)

``cd ~/.safegames``

9) Open the safegames.conf by typing nano safegames.conf then press i to go into insert mode and make the config look like this:

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
 externalip=VPS-IP-ADRESS
 masternodeprivkey=MASTERNODE_PRIKEY
 ```

Make sure to replace rpcuser and rpcpassword with a random username and password.

Insert your VPS IP-Adress and Masternode Privkey, you generated earlier.


10) to exit the editor press esc then CTRL+X and save the file

# Start your masternode
11) Now, you need to finally start these things in this order
– Start the daemon client in the VPS. First go back to your installed wallet directory, cd ~/safegames-1.0.0/bin and then start the wallet using ./sagegamesd

``cd ~/safegames-1.0.0/bin ``
``./sagegamesd``

– From the Control wallet debug consolestartmasternode alias false <mymnalias>
where <mymnalias> is the name of your masternode alias (without brackets)
The following should appear:
“overall” : “Successfully started 1 masternodes, failed to start 0, total 1”,
“detail” : [
{
“alias” : “<mymnalias>”,
“result” : “successful”,
“error” : “”
}
 
– Back in the VPS (remote wallet), start the masternode

``./safegames-cli startmasternode local false``

– A message “masternode successfully started” should appear

12)Use the following command to check status:

``./safegames-cli masternode status``

You should see something like:

{
“txhash” : “334545645643534534324238908f36ff4456454dfffff51311”,

“outputidx” : 0,

“netaddr” : “10.12.123.145:61555”,

“addr” : “S6fujc45645645445645R7TiCwexx1LA1”,

“status” : 4,

“message” : “Masternode successfully started”
}

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

