## GitHub SSH Auth/Signing

- Using MinTTy on windows, so environment variables might be different from your existing windows setup.
- Guide meant for windows, but should-not be any different in Linux, just ignore the powershell comands.

### 1 - Set Working Directory
`cd ~/.ssh`

`touch config`

### 2 - Creating & Adding Key

> Generate Key

`ssh-keygen -t ed25519 -C "orange@daraknet.cc"`

> In Powershell

`Get-Service -Name ssh-agent | Set-Service -StartupType Automatic` **(Elevated Command)**

`Start-Service ssh-agent`

> Then on Bash

`eval "$(ssh-agent -s)"`

> **OR** if want to continue in powershell, just do

Windows: `ssh-add C:\path\to\your\key`

Unix: `ssh-add ~/.ssh/key`

> Then, to verify changes do

`ssh-add -l`

_add .pub keys to GitHub, for Auth and Signing if want to sign using same key as well_

### 3 - Configuration

> ~/.ssh/config

```
Host OTonGitHub
  HostName github.com
  User git
  IdentityFile ~/.ssh/OTonGitHub
  AddKeysToAgent yes
  IdentitiesOnly yes
# LogLevel INFO | DEBUG
# Port 22
```

> Test Configuration

`ssh -T git@OTonGitHub`

> Set username and email

`git config --global user.name "OTonGitHub"`

`git config --global user.name orange@daraknet.cc`

- _quotes not necessary for email_

### 4 - Signing Commits

- First test if it works or not already.
- IF private email is disabled, just unset email, and make sure all staged commits don't contain private email.

`git config --global --unset user.email`

> IF staged commits contain private email, change it to provided no-reply email.

`git commit --amend --author="OTonGitHub <81725875+OTonGitHub@users.noreply.github.com>" --no-edit`

- Finally, enable signing commits, (to get rid of unverified commits)

`git config --global --unset gpg.format` *(skippable)*

`git config --global gpg.format ssh`

`git config --global user.signingkey ~/.ssh/OTonGitHub.pub`

`git config --global commit.gpgsign true`

### 5 - Adding Repository
#### New Repository
```
echo "# ProjectName" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:OTonGitHub/<REPOSITORY_NAME>.git
git push -u origin main
```

#### Push from an Existing Repository
```
git remote add origin git@github.com:OTonGitHub/<REPOSITORY_NAME>.git
git branch -M main
git push -u origin main
```
