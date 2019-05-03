How to fetch private submodules hosted on Github for a project?
===

## Challenge

Git client uses first SSH key in `ssh-agent`.

## Solutions

### Use key from a Github user that can access both repositories.

When this key is added as a configuration file and loaded in Semaphore yml,
the following lines pass:

```
git submodule init
git submodule update
```

### Use SSH config

If using the SSH key pair of a Github user is not an option, we can configure
SSH and Git in the following manner.
These lines map SSH key to a specified Github host.

```
# ~/.ssh/config
# config for first repo
Host github.com-repo-1
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_sem_1

# config for second repo
Host github.com-repo-2
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_sem_2

Host *github.com*
   StrictHostKeyChecking no
   UserKnownHostsFile=/dev/null
```

These lines update the SSH urls for repos.

```
# .gitmodules
[submodule "private_repo_1"]
	path = private_repo_1
	url = git@github.com-repo-1:mimimalizam/private_repo_1.git
[submodule "private_repo_2"]
	path = private_repo_2
	url = git@github.com-repo-2:mimimalizam/private_repo_2.git
```
