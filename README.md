# SSH/GitHub

- Using MinTTy on windows, so environment variables might be different.
## Commands
ssh-keygen -t ed25519 -C "your_email@example.com"
In powershell,
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent

Then on Bash,
My SSH Settings
eval "$(ssh-agent -s)"

Or if want to continue in powershell, just do
ssh-add C:\path\to\your\key

Then,
ssh-add -l
to verify.

Add .pub keys to GitHub, for Auth and Signing if want to sign using same key as well.

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

Test configuration
ssh -T git@OTonGitHub

Authenticatied!

set username and email
git config --global user.name "OTonGitHub"
git config --global user.name ot@myemail.com

Test Commit.
