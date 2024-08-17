# testing-hardware-key

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


# References

2024-08-17
https://www.yubico.com/blog/github-now-supports-ssh-security-keys/
