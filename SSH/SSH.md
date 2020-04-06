# SSH
## For Boot: 
```Shell
1) Check if Homebrew is installed on your Mac: open up a terminal window and type brew if it says “-bash: brew: command not found”, execute this: 
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```
```Shell
2) Install proxytunnel: 
brew install proxytunnel
```
```Shell
3) Make a file ~/.ssh/config 
nano ~/.ssh/config 
And put the following in it:
Host 192.168.11.* 192.168.90.*
User ana
Port 3128
KeepAlive yes
ProxyCommand proxytunnel -v -p dev.acit.com:3128 -P acit:HerpDerp -d %h:22
```
```Shell
4) ssh ana@192.168.11.98 should now work
password : apple2010
```
```Shell
For Port Mapping:
ssh -nNT -L 5555:localhost:5432 -l ana -i ~/PlanningTestInstance.pem refreshprod.artstor.acit.com
```
