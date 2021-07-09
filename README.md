# Easy Windows Git Bash SSH Configuration

Easy Windows SSH Git Bash Setup

`git clone` and copy contents to home folder

```
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
