# OpenStack


```Shell
dhcp-212-81:Downloads Zarak$ ssh-keygen -t rsa -b 4096 -f ccGroup30
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
