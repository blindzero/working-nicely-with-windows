# Git with multiple SSH Key on Windows (OpenSSH)

1. [Intro](#intro)
2. [TLDR;](#tldr;)
3. [Detailed](#detailed)
4. [Downsides](#downsides)

## Intro

For years we got used to having additional tools for using SSH on Windows clients (PuTTY as the most-common free, or OpenCRT as a commercial one).

Fortunately most common Git programms could make use of PuTTY to use it for Git-over-SSH (you just pointed the GIT_SSH system environment variable to PuTTY's plink.exe).

The big disadvantage was that Git-over-SSH didn't work out very well if you had loaded different SSH keys into PuTTY's authentication agent (Paegant). Git couldn't just select the right one.

Another big disadvantage PuTTY never used the standard (OpenSSH) public and private key, you always needed PuTTYgen program to copy & paste or export these formats out of PuTTY's own .PPK-file format.

So I personally was very curious what will happen now as Microsoft announced to implement the standard OpenSSH for Windows end of 2018.

## TLDR;

1. __Install Windows OpenSSH__ client _AND_ server component

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

2. __Enable__ and Start Windows __SSH agent__

```powershell
Set-Service -Name ssh-agent -StartupType 'Automatic'
Start-Service -Name ssh-agent
```

3. __Generate SSH Key__

```powershell
cd ~\.ssh
ssh-keygen
```

4. __Add SSH Key__

```powershell
ssh-add ~\.ssh\id_ed25519
```

5. __Point SSH Config to KEY for Git__

```ini
Host github.com
  HostName github.com
  User git
  IdentitiesOnly yes
  ForwardAgent yes
  IdentityFile "C:/Users/USERNAME/.ssh/id_ed25519"
```

## Detailed

## Downsides

## References

[1] [Microsoft's Installing OpenSSH with PowerShell](https://docs.microsoft.com/de-de/windows-server/administration/openssh/openssh_install_firstuse)

[2] [Configure Windows OpenSSH](https://docs.microsoft.com/de-de/windows-server/administration/openssh/openssh_server_configuration)

[3] [OpenSSH Key Management](https://docs.microsoft.com/de-de/windows-server/administration/openssh/openssh_keymanagement)