# ArgoCoin MasterNode Setup Guide
VER - 1.0.0.1      # 2018.02.19  

I have done my best to guide the argo master node to work perfectly.  
Especially I have tried to avoid the **"WATCHDOG_EXPIRED"** error.

# Youtube video
soon update.  

# Desktop wallet setting
1. Open the ArgoCoin Desktop Wallet.  
2. Go to **[Receive]** -> Label: **MN1** -> Amount: 10000 -> **[Request payment]**  
3. Send ***10000*** Argo to **MN1**  
4. Wait for 20 confirmations.  
5. Go to **[Help]** -> **[Debug Window - Console]**  
6. Type the following command: **masternode outputs**  
output:  
{ "c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c": "0" }  
remember. "c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c" is your [tx_hash]  
remember. "0" is your [tx_index]  
7. Type the following command: **masternode genkey**  
output:  
65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW  
remember. 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW is your [masternodePrivkey]  
8. Go to **[Tools] -> [Open Masternode Configuration File]**  
9. edit **masternode.conf** file:  
* **mn1 [vps_ip]:8989 [masternodePrivkey] [tx_hash] [tx_index]**  
ex) mn1 197.4.0.21:8989 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c 0
10. save **masternode.conf** file  

# Multiple MN on one VPS:
1. sdjffsdajlk  
2. asfdljsfdjkl  
3. fsdjklfsdajlk  

# Explanation of term
mn1 127.0.0.2:19999 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c 0

masternode outputs -> **[tx_hash] [tx_index]**  
ex) c84f87000000000083000000658810b92e0d032zz3c840000f9e18714456a67c 0  
masternode genkey -> **[masternodePrivkey]**  
ex) 65WitritDkin0000002V0000000000b65SsCPk3eeMaaL1KHinW  
UBuntu IP Address -> **[vps_ip]**  
rpc port -> 8988 fixed  
masternode port -> 8989 fixed.  

# Any donation is highly appreciated
**AROG**: ARk1DQm5aiGuHkYf3yyYZruCYBz53dywo8  
**BTC**: 3CcatZ6hAeBb9jMc4SV2eDEVpKLUSJfDoQ  
**ETH**: 0x3cc9e0cdd69added605f8c9c980e35c2dedfb4f3  

# Thank you.
