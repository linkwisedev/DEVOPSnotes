## DEVOPS notes 2024
### SSH key generation

Generate SSH public and private keys

```
ssh-keygen -t ed25519 -C "key name"
```

Add SSH key to server

```
ssh-copy-id -i ~/.ssh/name_of_key.pub 172.16.250.133
```
