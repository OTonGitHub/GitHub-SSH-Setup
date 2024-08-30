# GitHub SSH Auth/Sign
- Using MinTTy on windows, so environment variables might be different from your existing windows setup.

## Commands
> Generate Key

```ssh-keygen -t ed25519 -C "your_email@example.com"```
> In Powershell

```Get-Service -Name ssh-agent | Set-Service -StartupType Automatic```

```Start-Service ssh-agent```

> Then on Bash

```eval "$(ssh-agent -s)"```

> **OR** if want to continue in powershell, just do

```ssh-add C:\path\to\your\key```

> Then, to verify changes do

```ssh-add -l```

*add .pub keys to GitHub, for Auth and Signing if want to sign using same key as well*

## Configuration
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

```ssh -T git@OTonGitHub```

> Set username and email

`git config --global user.name "OTonGitHub"`

`git config --global user.name your_email@example.com`
- *quotes not necessary for email*

## Signing Commits
- first test if it works or not already.

