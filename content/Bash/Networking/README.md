# Networking



### Get local machine's IP
```bash
ip a | grep "inet 192" | awk '{print $2}'
```