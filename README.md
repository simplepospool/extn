# ExtensiveCoin
Shell script to install a [ExtensiveCoin Masternode](http://extensivecoin.com/) on a Linux server running Ubuntu 14.04 or 16.04. Use it on your own risk.

***
## Installation:
```
1) wget -q https://raw.githubusercontent.com/extensivecoin/ExtensiveCoin_MN_Installer/master/EXTN_MN_Installer.sh
2) bash EXTN_MN_Installer.sh
```
***

## Desktop wallet setup

After the MN is up and running, you need to configure the desktop wallet accordingly. Here are the steps for Windows Wallet
1. Open the ExtensiveCoin Desktop Wallet.
2. Go to RECEIVE and create a New Address: **MN1**
3. Send **350** **EXTN** to **MN1**.
4. Wait for 6 confirmations.
5. Go to **Tools -> "Debug console - Console"**
6. Type the following command: **masternode outputs**
7. Go to  ** Tools -> "Open Masternode Configuration File"
8. Add the following entry:
```
Alias Address Privkey TxHash Output_index
```
* Alias: **MN1**
* Address: **VPS_IP:PORT**
* Privkey: **Masternode Private Key**
* TxHash: **First value from Step 6**
* Output index:  **Second value from Step 6**
9. Save and close the file.
10. Go to **Masternode Tab**. If you tab is not shown, please enable it from: **Settings - Options - Wallet - Show Masternodes Tab**
11. Click **Update status** to see your node. If it is not shown, close the wallet and start it again. Make sure the wallet is unlocked.
12. Open **Debug Console** and type:
```
startmasternode "alias" "0" "MN1"
```
***

## Usage:
```
extn-cli mnsync status
extn-cli getinfo
extn-cli masternode status
```

Also, if you want to check/start/stop **EXTN** , run one of the following commands as **root**:

**Ubuntu 16.04**:
```
systemctl status EXTN #To check the service is running.
systemctl start EXTN #To start EXTN service.
systemctl stop EXTN #To stop EXTN service.
systemctl is-enabled EXTN #To check whetether EXTN service is enabled on boot or not.
```
**Ubuntu 14.04**:  
```
/etc/init.d/EXTN start #To start EXTN service
/etc/init.d/EXTN stop #To stop EXTN service
/etc/init.d/EXTN restart #To restart EXTN service
```

***

**./extnd: error while loading shared libraries: libboost_system.so.1.58.0: cannot open shared object file: No such file or directory**: 

Because the standard version of libboost  libboost 1.58. we shall installing libboost 1.58 manually. follow this command:
```
sudo apt-get install g++ python-dev autotools-dev libicu-dev libbz2-dev
wget -O boost_1_58_0.tar.gz https://sourceforge.net/projects/boost/files/boost/1.58.0/boost_1_58_0.tar.gz/download
tar -xvzf boost_1_58_0.tar.gz
cd boost_1_58_0/

./bootstrap.sh --prefix=/usr/local
user_configFile=`find $PWD -name user-config.jam`
echo "using mpi ;" >> $user_configFile
n=`cat /proc/cpuinfo | grep "cpu cores" | uniq | awk '{print $NF}'`
sudo ./b2 --with=all -j $n install 
sudo sh -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/local.conf'
sudo ldconfig
```



