# Rabbit MQ

## List Users: 
```Shell
sudo rabbitmqctl list_users
```
## List Queues : 
```Shell
sudo rabbitmqctl list_queues
```
## Add User: 
```Shell
sudo rabbitmqctl add_user work work
sudo rabbitmqctl set_user_tags work
sudo rabbitmqctl add_vhost work
sudo rabbitmqctl set_permissions -p work work ".*" ".*" ".*"
```

## Show Report :
```Shell
sudo rabbitmqctl report
```
https://www.rabbitmq.com/api-guide.html