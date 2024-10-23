---
title: "SSH"
description: ""
summary: ""
date: 2024-10-08T23:15:45-07:00
lastmod: 2024-10-08T23:15:45-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

The public/private key system is an asymmetric encryption method used in SSH (Secure Shell) to authenticate and secure communication between a client and a server.

- **Private Key**: This key stays on your local machine (client) and should be kept secure and secret. It is used to decrypt messages encrypted with the corresponding public key and to sign messages that prove your identity.
- **Public Key**: This key is shared with the remote server. It can be freely distributed because it cannot decrypt messages by itself. When the server receives a message signed by the private key, it can verify it using the public key, confirming the client’s identity.

{{< callout context="tip" icon="outline/rocket" >}}
The public key is stored on the server. The private key is stored on your local machine (the client).
{{< /callout >}}

## Create SSH Key

The `ssh-keygen` tool can be used to create a SSH key pair. The key pair is generated on your computer (the client). The public key is then uploaded to the target server.

```bash
ssh-keygen
ssh-keygen -t rsa
ssh-keygen -b 4096
ssh-keygen -C "greg@example.com"
```

- `-t rsa`: Specifies the RSA algorithm (widely used and secure).
- `-b 4096`: Generates a 4096-bit key for stronger security.
- `-C "greg@example.com"`: An optional comment field, useful for identifying the key.

### Create optional passhrase

A SSH key can additionally be updated with a passphrase for additional secrutiy. This passphrase will be needed every time the key pair is used.

## Upload SSH Key to Remote Server

The public key needs to be added to the remote server's `~/.ssh/authorized_keys` file. The public key can be added with the `ssh-copy-id` command or by adding the public key directly to the `~/.ssh/authorized_keys` file.

Use the `ssh-copy-id` command to send the newly created private key to the remote host. The remote host needs a user with assigned password to send the private key.

```bash
ssh-copy-id USERNAME@SERVER_IP
```

The private key stays on the client and is not uploaded to the server.

## Update Permissions on Server

On the server, verify the following permissions are set:

```bash { title="Update key permissions on remote server" }
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

## Connect to Remote Server

Now you can log into the server without a password, using the private key instead.

## Disable SSH Password Authentication

Disable users from accessing a server with SSH based auth.

```bash
sudo vim /etc/ssh/sshd_config
sudo vim /etc/ssh/sshd_config.d/50-cloud-init.conf
sudo systemctl restart ssh
```
