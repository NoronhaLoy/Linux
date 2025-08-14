# 🚀 SSH Cheat Sheet — SRE / DevOps / Platform Engineer

## 1️⃣ Basic SSH Usage

| Command | Description (with parameter hints) |
|---------|------------------------------------|
| `ssh username@hostname_or_ip` | Connect to a remote server. Replace `username` with your login user, and `hostname_or_ip` with the server's domain or IP. |
| `ssh -p 2222 username@hostname` | Connect on a non-standard port. Replace `2222` with the actual SSH port. |
| `ssh -i ~/.ssh/id_rsa username@host` | Use a custom private key file. Useful for multiple keys. |
| `ssh -C username@host` | Enable compression for slow connections. |

---

## 2️⃣ Key Generation & Management

| Command | Description (with parameter hints) |
|---------|------------------------------------|
| `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"` | Generate a 4096-bit RSA key with a comment (usually email). |
| `ssh-keygen -t ed25519 -C "your_email@example.com"` | Generate a modern ED25519 key (faster, secure). |
| `ssh-keygen -f ~/.ssh/my_key -t rsa -b 4096` | Generate key pair and store in `my_key` instead of default. |
| `eval $(ssh-agent)` | Start SSH agent in current shell. |
| `ssh-add ~/.ssh/my_key` | Add a specific private key to the SSH agent. |
| `ssh-copy-id -i ~/.ssh/my_key.pub username@host` | Copy public key to remote host for passwordless login. |

---

## 3️⃣ File Transfer (SCP, Rsync, SFTP)

| Command | Description (with parameter hints) |
|---------|------------------------------------|
| `scp file.txt user@host:/remote/path/` | Copy file from local → remote. |
| `scp user@host:/remote/file.txt ./` | Copy file from remote → local. |
| `scp -P 2222 file.txt user@host:/remote/path/` | Copy via a non-standard SSH port. |
| `scp -i ~/.ssh/key.pem file user@host:/path/` | Copy using a specific SSH key. |
| `scp -r folder/ user@host:/remote/path/` | Recursively copy a folder. |
| `rsync -avz -e ssh /local/path user@host:/remote/path` | Sync files with compression. |
| `sftp user@host` | Start interactive SFTP session. |
| `sftp -i ~/.ssh/key.pem user@host` | SFTP with specific key. |

---

## 4️⃣ Port Forwarding & Tunneling

| Command | Description (with parameter hints) |
|---------|------------------------------------|
| `ssh -L 8080:localhost:80 user@host` | Local forwarding: access remote port 80 at `localhost:8080`. |
| `ssh -R 9090:localhost:3000 user@host` | Remote forwarding: make your local service (3000) available on remote (9090). |
| `ssh -D 1080 user@host` | Dynamic SOCKS proxy (use in browsers with `127.0.0.1:1080`). |
| `ssh -L 3306:db.example.com:3306 user@bastion` | Tunnel to database through a bastion host. |

---

## 5️⃣ Multi-Hop & Jump Hosts

| Command | Description (with parameter hints) |
|---------|------------------------------------|
| `ssh -J jump_user@jump_host target_user@target_host` | Connect via jump host. |
| `ssh -J bastion_user@bastion:22 target_user@internal` | Jump via bastion to internal server. |
| `ssh -o ProxyCommand="ssh -W %h:%p jump_user@jump_host" target_user@target_host` | Legacy jump host method. |

---

## 6️⃣ .ssh/config File — Power User Setup

Create/edit `~/.ssh/config` to simplify commands:  

```ssh-config
Host myserver
    HostName example.com
    User deploy
    Port 2222
    IdentityFile ~/.ssh/my_key
    ForwardAgent yes
```

Then connect simply with:

```bash
ssh myserver
```

**Tips for `.ssh/config`**  
- **`Host`** → Nickname for the connection (can be wildcard like `Host *.example.com`).  
- **`HostName`** → Actual IP or DNS name.  
- **`User`** → Default username.  
- **`Port`** → Custom port if not 22.  
- **`IdentityFile`** → Path to private key.  
- **`ForwardAgent`** → For agent forwarding.  
- **`ProxyJump`** → Jump/bastion host configuration.  
- **`ServerAliveInterval`** & `ServerAliveCountMax` → Keep session alive.  
- **`StrictHostKeyChecking no`** → Disable key check (use cautiously).  

---

## 7️⃣ SSH Certificates & Security (SRE/DevOps Context)

| Task | Command / Notes |
|------|-----------------|
| Generate self-signed SSH CA | `ssh-keygen -f ssh_ca -t rsa -b 4096` |
| Sign a user's public key with CA | `ssh-keygen -s ssh_ca -I user_cert -n username user_key.pub` |
| Use a certificate in `.ssh/config` | `CertificateFile ~/.ssh/user_cert.pub` |
| View certificate details | `ssh-keygen -L -f user_cert.pub` |
| Restrict commands with key | In `authorized_keys`: `command="/path/to/script"` before key. |
| Disable password login | `/etc/ssh/sshd_config`: `PasswordAuthentication no` |
| Enable key login only | `/etc/ssh/sshd_config`: `PubkeyAuthentication yes` |
| Rotate keys regularly | Store expiry in cert and regenerate. |

---

## 8️⃣ Troubleshooting

| Command | Description |
|---------|-------------|
| `ssh -v user@host` | Verbose mode (debug SSH connection issues). |
| `ssh -vvv user@host` | Very verbose (packet level debug). |
| `ssh-keyscan host` | Get public host key (for known_hosts). |
| `ssh -o StrictHostKeyChecking=no user@host` | Skip key checking (use for automation scripts cautiously). |
| `tail -f /var/log/auth.log` | View SSH auth logs on remote Linux server. |

---

✅ **Pro Tip for Git Usage:**  
Store this file as `ssh_cheatsheet.md` in your repo, and you can copy any command by clicking the code block’s copy button in GitHub UI.  

---

