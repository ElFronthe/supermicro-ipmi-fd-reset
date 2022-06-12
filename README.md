# supermicro-ipmi-fd-reset
reset supermicro ipmi to factory default and bring network settings back
```
wget https://mirror.galaxydata.ru/supermicro/IPMICFG_1.30.0_build.190710.zip
unzip IPMICFG_1.30.0_build.190710.zip
ln -s /home/hostlivekey/IPMICFG_1.30.0_build.190710/Linux/64bit/IPMICFG-Linux.x86_64 /usr/local/sbin/ipmicfg
ipmiIP=$(ipmicfg -m | awk '/IP=/' | cut -b 4-)
ipmiMASK=$(ipmicfg -k | awk '/Subnet Mask=/' | cut -b 13-)
ipmiGW=$(ipmicfg -g | awk '/Gateway=/' | cut -b 9-)
echo "IP $ipmiIP"
echo "mask $ipmiMASK"
echo "gw $ipmiGW"
ipmicfg -fd
echo "Wait 20 sec"
sleep 20
ipmicfg -dhcp on
ipmicfg -dhcp off
ipmicfg -m $ipmiIP
sleep 3
ipmicfg -k $ipmiMASK
sleep 3
ipmicfg -g $ipmiGW
sleep 3
ipmicfg -user setpwd 2 your-super-strong-password
echo "TEST IPMI BEFORE EXIT"
```
