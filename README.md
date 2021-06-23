# gitbashssh
Easy Windows SSH Git Bash Setup

```
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add -K ~/.ssh/id_ed25519
```
