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

## Login Logs

### See login history

Use `last` to see list of previously logged in users. This list shows users that have successfully logged in.

```bash { title="Show last logged in users" }
last
```

```bash { title="Last command with output" }
$ last

admin    pts/0        172.16.1.108     Wed Oct 23 11:44    gone - no logout
admin    pts/0        172.16.1.89      Tue Oct 22 23:23 - 00:21  (00:57)
admin    pts/0        172.16.1.216     Sun Oct 13 11:31 - 19:19  (07:48)
```

### See Authentication Attempts

The `/var/log/auth` file contains system authorization information, including user logins and authentication machinsm that were used. Failed authentication attempts will appear in this list

```bash { title="Show authorization history" }
cat /var/log/auth.log
```

```bash
$ cat /var/log/auth.log

2024-10-23T11:44:49.860344-07:00 machine sshd[4038254]: Accepted publickey for ubuntu from 172.16.5.119 port 49951 ssh2: RSA SHA256:XXXXXXXXXX
2024-10-23T11:57:26.968682-07:00 machine sshd[4038387]: Received disconnect from 172.16.5.119 port 49951:11: disconnected by user
2024-10-23T11:57:26.978035-07:00 machine sshd[4038387]: Disconnected from user ubuntu 172.16.5.119 port 49951
2024-10-23T11:59:01.556874-07:00 machine CRON[4038805]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
2024-10-23T11:59:01.560481-07:00 machine CRON[4038805]: pam_unix(cron:session): session closed for user root
2024-10-24T23:12:29.780820-07:00 machine sshd[2494962]: Invalid user blank from 83.16.188.111 port 38888
2024-10-24T23:12:30.030242-07:00 machine sshd[2494962]: Connection closed by invalid user blank 83.16.188.111 port 38888 [preauth]
2024-10-25T23:12:58.894637-07:00 machine sshd[2495950]: Connection closed by authenticating user root 45.45.16.190 port 45630 [preauth]
```

## See All Users Associated with Running Processees

List all users that have initiated a system process.

```bash { title="Show list of user ids running processes" }
ps -eo user | sort | uniq -c
```

```bash { title="Output of command" }
$ ps -eo user | sort | uniq -c
      5 dhcpcd
     45 ubuntu
      1 polkitd
    116 root
      1 _rpc
      7 sshd
      1 statd
      1 syslog
      1 telegraf
      1 USER
      9 www-data
```
