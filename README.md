## Download the ethercat master source files:

$ wget http://www.etherlab.org/download/ethercat/ethercat-1.5.2.tar.bz2

$ tar xjf ethercat-1.5.2.tar.bz2

## Get Ethernet infomation

$ sudo lspci -v 

$ cd ./ethercat-1.5.2/

$ ./configure --disable-8139too --enable-e1000e

$ make

$ make modules

$ sudo make install

$ sudo make modules_install

$ sudo /sbin/depmod

## Configure your environnement

$ sudo mkdir /etc/sysconfig

$ sudo cp scripts/sysconfig/ethercat /etc/sysconfig

$ sudo cp scripts/init.d/ethercat /etc/init.d/ethercat

$ sudo nano /etc/sysconfig/ethercat

In the file, enter the Ethernet devices Mac address in the MASTER0_DEVICE variable and the driver in DEVICE_MODULES variable

	MASTER0_DEVICE="e8:40:f2:3e:73:b4"

	DEVICE_MODULES="e1000e"

## The EtherCAT master service can now be started, restarted and stopped using: 

sudo /etc/init.d/ethercat start

sudo /etc/init.d/ethercat restart

sudo /etc/init.d/ethercat stop


## To run the service without root, a rule file has to be created*: 

cd /etc/udev/rules.d/

nano 98-EtherCAT.rules

In the file add the line: “KERNEL==“EtherCAT[0-9]* “ , MODE=”0664” , GROUP=”users”. 

## Alternatively, to run the service without root, type the following line in the command prompt:

echo 'KERNEL=="EtherCAT[0-9]*" , MODE="0664" , GROUP = "users"' | sudo tee /etc/udev/rules.d/98-EtherCAT.rules

## Start EtherCAT master at startup

sudo chmod a+x /etc/init.d/ethercat

sudo update-rc.d ethercat start 51 S .


