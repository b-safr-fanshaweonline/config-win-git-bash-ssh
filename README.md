# Configure Windows Git Bash With SSH

## Create a new SSH key

[Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-windows)

### Generate Key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Note: If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### Save Key

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
## Test

```
# start the ssh-agent in the background
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```
```
$ ssh-add ~/.ssh/id_ed25519
```

## Add to GitHub Account

Add the SSH key to your account on GitHub. For more information, see "[Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)".

## Automate

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
test -f ~/.profile && . ~/.profile
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
