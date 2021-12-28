# Configure Windows Git Bash With SSH

## 1.Create a new SSH key

[Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-windows)

### 1.1 Generate Key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Note: If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### 1.2 Save Key

```
> Generating public/private algorithm key pair.
```
```
> Enter a file in which to save the key (/c/Users/you/.ssh/id_algorithm):[Press enter]
```
```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

### 1.3 Test

```
# start the ssh-agent in the background
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```
```
$ ssh-add ~/.ssh/id_ed25519
```

## 2. Add to GitHub Account

[Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

### 2.1 Copy key to clipboard

```
C:\Users\you\.ssh\id_ed25519
```

```
file:///C:/Users/you/.ssh/id_ed25519
```

### 2.2 Add New Key

[![](https://docs.github.com/assets/cb-24835/images/help/settings/ssh-key-paste.png)](https://github.com/settings/ssh/new)

[SSH keys / Add new](https://github.com/settings/ssh/new)

## 3. Automate

Copy to your home folder (C:\Users\you\):
- `.ssh/config`
- `.bash_profile`
- `.bashrc`

### `.ssh/config`

```
Host github.com
 Hostname github.com
 IdentityFile ~/.ssh/id_ed25519
```

### `.bash_profile`

```sh
test -f ~/.bashrc && . ~/.bashrc
```

### `.bashrc`

```sh
# Start SSH Agent
#----------------------------

SSH_ENV="$HOME/.ssh/environment"

function run_ssh_env {
  . "${SSH_ENV}" > /dev/null
}

function start_ssh_agent {
  echo "Initializing new SSH agent..."
  ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
  echo "succeeded"
  chmod 600 "${SSH_ENV}"

  run_ssh_env;

  ssh-add ~/.ssh/id_ed25519;
}

if [ -f "${SSH_ENV}" ]; then
  run_ssh_env;
  ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
    start_ssh_agent;
  }
else
  start_ssh_agent;
fi
```

[Working with SSH key passphrases](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases)
