# open-git-bash
$ ssh-keygen -t rsa -b 4096 -C "pricillapburks@gmail.com"
> Generating public/private rsa key pair.
$ ssh-keygen -p

# Start the SSH key creation process
> Enter file in which the key is (/Users/DuCcy/.ssh/id_rsa): [Hit
enter]

> Key has comment '/Users/DuCcy/.ssh/id_rsa'
> Enter new passphrase (empty for no passphrase): [ssh-keys]
> Enter same passphrase again: [ssh-keys]
> Your identification has been saved with the new passphrase.
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
# start the ssh-agent in the background
$ eval $(ssh-agent -s)
> Agent pid 59566
$ ssh-add ~/.ssh/id_rsa
