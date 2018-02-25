# ArgoCoin MasterNode Setup Guide

I have done my best to guide the argo masternode to work perfectly.  
Especially I have tried to avoid the **"WATCHDOG_EXPIRED"** error.

# Youtube video
https://youtu.be/lh1lI5VENKg  

# Desktop wallet setting
Download - [Wallets for Windows x32 x64](https://argo.cash/)  
1. Open the ArgoCoin Desktop Wallet. (your local Windows wallet.)  
2. Go to **[Receive]** -> Label: **MN1** -> Amount: 10,000 -> **[Request payment]**  
3. Send exactly **10,000** Argocoins to **MN1** address  
4. Wait for 15 confirmations.  
5. Go to **[Help]** -> **[Debug Window - Console]**  
6. Type the following command: **masternode outputs**  
  
    { "c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c": "0" }      
**REMEMBER** "c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c" is your **[tx_hash]**  
**REMEMBER** "0" is your **[tx_index]**  
  
7. Type the following command: **masternode genkey**  
  
    65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW      
**REMEMBER** 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW is your **[masternodePrivkey]**  
  
8. Go to **[Tools] -> [Open Masternode Configuration File]**  
9. edit **masternode.conf** file:  
  **mn1 [vps_ip]:8989 [masternodePrivkey] [tx_hash] [tx_index]**  
ex)  
    mn1 197.4.0.21:8989 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c 0
10. save **masternode.conf** file  

# Explanation of term
masternode outputs -> **[tx_hash] [tx_index]**  
  
    c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c 0
masternode genkey -> **[masternodePrivkey]**  
  
    65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW
UBuntu IP Address -> **[vps_ip]**  
rpc port -> 8988 fixed  
masternode port -> 8989 fixed.  

# VPS Setting 
DigitalOcean: [www.digitalocean.com](https://m.do.co/c/08f956ba58f6)  
Vultr: [www.vultr.com](https://www.vultr.com/?ref=7335357)  
  
1. Login VPS. ( DigitalOcean or Vultr )  
2. Create Droplets. (Deploy New Instance.)  
3. Server Type: **Ubuntu 16.04 x64**  
4. Server Size: 1CPU - 2GB(2048MB) MEMORY  
5. Create. (Deploy Now)  

# Bitvise SSH Client Setting
Download: [https://www.bitvise.com/ssh-client-download](https://www.bitvise.com/ssh-client-download)  
  
1. Dowanload [Bitvise SSH Client installer - version x.xx, size xx.x MB.]
2. Server Host is Your **[vps_ip]**  
3. Server Port is 22  
4. Username is **root**
5. Login.
6. "Host Key Verification" Message -> **Accecpt and Save**  
  
# Enable Firewall
$sudo apt-get update  
$sudo apt-get upgrade  
  
    y
    
$sudo apt-get install ufw  
$sudo ufw allow ssh  
$sudo ufw limit ssh/tcp  
$sudo ufw allow 8988/tcp  
$sudo ufw allow 8989/tcp  
$sudo ufw default allow outgoing  
$sudo ufw enable  
  
    y
  
$sudo ufw status  
  
$sudo apt-get install fail2ban  
$systemctl enable fail2ban  
$systemctl start fail2ban  
  
# Masternode Setting on VPS
$adduser argo  
  
    input your PASSWORD
  
$gpasswd -a argo sudo  
$su - argo  
$sudo add-apt-repository -y ppa:bitcoin/bitcoin  
$sudo apt-get update  
$sudo apt-get upgrade  
$sudo apt-get dist-upgrade  
$sudo apt-get install -y software-properties-common nano libboost-all-dev libzmq3-dev libminiupnpc-dev libssl-dev libevent-dev libdb4.8-dev libdb4.8++-dev  
$mkdir ~/.argocore  
$touch ~/.argocore/argo.conf  
$touch ~/.argocore/masternode.conf  
$nano ~/.argocore/argo.conf  
  
    rpcuser=argocoinuser
    rpcpassword=argocoinpass
    rpcport=8988
    rpcallowip=127.0.0.1
    listen=1
    server=1
    daemon=1
    masternode=1
    masternodeprivkey=[masternodePrivkey]
  
[masternodePrivkey] is your masternodePrivkey.  
**SAVE** argo.conf file  
  
  
$nano ~/.argocore/argo.conf  
**[Important!!]** Please check the argo.conf file again!!  
  
$nano ~/.argocore/masternode.conf   
  
    mn1 197.4.0.21:8989 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c 0
  
**mn1 [vps_ip]:8989 [masternodePrivkey] [tx_hash] [tx_index]** 

**SAVE** masternode.conf file  
  
  
$mkdir argo && cd argo  
$wget https://github.com/argocoins/argo/releases/download/v1.0.0/argo-ubuntu1604-v1.0.0.tar.gz  
$tar -xvf argo-ubuntu1604-v1.0.0.tar.gz  
$./argod  
$./argo-cli mnsync status | grep IsSynced  
  
    Wait for ["IsSynced": true"] (about 10 min.)
  
If "IsSynced" is "false", please don't do anything and just wait for ["IsSynced":true].  
  
[" IsSynced":true "] takes more than 10 minutes.  
  
# Go to Desktop wallet  
1. Open the ArgoCoin Desktop Wallet.  
2. Go to **[Settings]** -> **[Options]** ->**[Wallet]**  
Enable the "Enable coin control features" and "Show Masternodes Tab"  
  
3. Restart Desktop wallet.  
4. **[Masternodes]** -> **[Start all]**  
**Warning** - Be sure to click [Start all] only when "IsSynced" is "true".  
5. **[Masternodes]** -> **[Update status]**  
  
# Argo Sentinel Setting
reference: https://github.com/argocoins/sentinel  
  
$su - argo  
$sudo apt-get install -y git python-virtualenv  
$cd ~/argo  
$sudo apt-get update  
$sudo apt-get -y install virtualenv  
$./argo-cli getinfo | grep version  
$git clone https://github.com/argocoins/sentinel.git && cd sentinel  
$nano sentinel.conf  
  
    argo_conf=/home/argo/.argocore/argo.conf
  
**ADD** and **SAVE** sentinel.conf file  
  
$virtualenv ./venv  
$./venv/bin/pip install -r requirements.txt  
$./venv/bin/py.test ./test  
  
    There should be NO ERROR.
  
$./venv/bin/python bin/sentinel.py  
  
    There should be NO ERROR.
  
If you get ERROR, check $nano ~/.argocore/argo.conf  
  
$chmod -R 755 database  
$crontab -e  
  
    * * * * * cd /home/argo/argo/sentinel && ./venv/bin/python bin/sentinel.py >> sentinel.log 2>&1
  
**ADD** and **SAVE** crontab file  
  
Please wait about 20 minutes.  
  
# Restart Desktop wallet  
[Masternodes]-[Status]-[ENABLED] ==> You will get ARGO coins after about 10 hours.  
  
  
  
If you continue to occurs WATCHDOG_EXPIRED error, check the following:  
$su - argo  
$cd ~/argo  
$./argo-cli mnsync status | grep IsSynced  
  
    "IsSynced": true,
  
$./argo-cli masternode status | grep status  
  
    "status": "Masternode successfully started"
  
$cd ~/argo/sentinel  
$./venv/bin/py.test ./test  
  
    ===== 23 passed in 0.25 seconds =====
  
$./venv/bin/python bin/sentinel.py  
  
    There should be NO ERROR.
  
# Any donation is highly appreciated


| Coin           | Address  |
| ---------------| ---------|
| AROG | ARk1DQm5aiGuHkYf3yyYZruCYBz53dywo8 |
| BTC 			 | 3CcatZ6hAeBb9jMc4SV2eDEVpKLUSJfDoQ  |
| ETH 			 | 0x3cc9e0cdd69added605f8c9c980e35c2dedfb4f3 |
| LTC 			 | 3FF59A9xmfyhvmz5Zi2pUah9J7VVDVXyrL |

	
# Thank you.  
