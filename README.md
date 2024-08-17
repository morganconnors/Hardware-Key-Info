# Hardware Key Info

# Create SSH key

## Pre-reqs

Install OpenSSH and libfido2. On linux, OpenSSH comes default (most distros) and libfido2 can be installed on it's own or by installing ```yubico-piv-tool``` and ```yubikey-manager``` it will be installed with the yubikey tools. 

## Generating the key

I will generate a resident key because my yubikey supports it.

```
ssh-keygen -t ecdsa-sk -O resident
```

This creates a new private key and public key in your ssh directory. In order to use the ssh key on new computers, you must add it to the new device.

## Adding the SSH key to a new device

```
ssh-add -K
```

This loads a key handle into the SSH agent to make the key available to the new computer. You must do that every time the computer reboots.

Alternatively, for a more permanent solution you can run:  
```
ssh-keygen -K
```

This generates two files in the current directory: ```id_ecdsa_sk_rk``` and ```id-ecdsa_sk_rk.pub```. Rename the private key to ```id_ecdsa_sk``` and move it to your SSH directory.


## References

2024-08-17
[Yubico Website](https://www.yubico.com/blog/github-now-supports-ssh-security-keys/)

# Signing git commits with SSH keys

## Pre-reqs

Git 2.34.0 or newer
OpenSSH 8.1 or newer

Generate a SSH key or use an existing one.

Add it to your Github/Gitlab/whatever account if necessary.

## Configuration

Configure git to use SSH for signing.

```
git config --global gpg.format ssh
```

Specify the public key to use as the signing key

```
git config --global user.signingkey [key location]
```

## Signing

Make a new commit.

```
git commit -S -m "Commit message"
```
-S is the signing flag.

Alternatively, if you don't want to use the ```-S``` flag every time, you can tell git to sign automatically. 
```
git config --global commit.gpgsign true
```

## Important Note

When doing this, especially with Github, you need to make sure you successfully configured your user name and user email or else Github might mark the commit as unverified.

```
git config --global user.name "[Name]"
git config --global user.email "[Email"
```

## References

2024-08-17 [Gitlab](https://docs.gitlab.com/ee/user/project/repository/signed_commits/ssh.html)
