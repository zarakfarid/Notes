# OpenStack


```Shell
dhcp-212-81:Downloads Zarak$ ssh-keygen -t rsa -b 4096 -f ccGroup30
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in ccGroup30.
Your public key has been saved in ccGroup30.pub.
The key fingerprint is:
ba:83:59:41:e7:fd:99:1e:da:f1:77:e5:aa:52:46:24 Zarak@dhcp-212-81.vpn.tu-berlin.de
The key's randomart image is:
+--[ RSA 4096]----+
|                 |
|      . . E .    |
|     . o . o     |
|      . . . .    |
|       .S  o o   |
|      ..    O   .|
|     +.    * + ..|
|    o ..  o o . +|
|      ..   ....o.|
+-----------------+
```
## For Boot: 
```Shell
nova boot --flavor 'Cloud Computing' --image ubuntu-16.04 --key-name ccGroup30 --availability-zone 'Cloud Computing 2017' --security-groups default --nic net-name=cc17-net ccGroup30Instance
```
## For Ping: 
```Shell
openstack security group rule create defualt --protocol icmp --remote-ip 0.0.0.0/0
```

## For SSH: 
```Shell
openstack security group rule create --protocol tcp --dst-port 22:22 --remote-ip 0.0.0.0/0 default
```