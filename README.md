## GitHub SSH Auth/Sign

- Using MinTTy on windows, so environment variables might be different from your existing windows setup.
- Tested on windows, but should-not be any different in Linux, just ignore the powershell comands.

### Commands

> Generate Key

`ssh-keygen -t ed25519 -C "your_email@example.com"`

> In Powershell

`Get-Service -Name ssh-agent | Set-Service -StartupType Automatic`

`Start-Service ssh-agent`

> Then on Bash

`eval "$(ssh-agent -s)"`

> **OR** if want to continue in powershell, just do

`ssh-add C:\path\to\your\key`

> Then, to verify changes do

`ssh-add -l`

_add .pub keys to GitHub, for Auth and Signing if want to sign using same key as well_

### Configuration

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

`git config --global user.name your_email@example.com`

- _quotes not necessary for email_

### Signing Commits

- First test if it works or not already.
- IF private email is disabled, just unset email, and make sure all staged commits don't contain private email.

`git config --global --unset user.email`

> IF staged commits contain private email, change it to provided no-reply email.

`git commit --amend --author="OTonGitHub <81725875+OTonGitHub@users.noreply.github.com>" --no-edit`

- Finally, enable signing commits, (to get rid of unverified commits)

`git config --global --unset gpg.format`

`git config --global gpg.format ssh`

`git config --global user.signingkey ~/.ssh/OTonGitHub.pub`

`git config --global commit.gpgsign true`
