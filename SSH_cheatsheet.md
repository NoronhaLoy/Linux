# SSH Commands Cheat Sheet

| Command | Description |
|---------|-------------|
| `ssh username@hostname_or_ip` | Basic SSH connection to a remote server. Prompts for password. |
| `ssh-keygen -t rsa` | Generates an SSH key pair for passwordless authentication. |
| `ssh-copy-id username@hostname_or_ip` | Copies your public key to a remote server to enable passwordless login. |
| `scp /path/to/local/file username@host:/remote/path` | Copies a file from local to remote. |
| `scp username@host:/remote/path/file.txt /local/path/` | Copies a file from remote to local. |
| `scp -r /local/path username@host:/remote/path` | Copies a local directory to a remote server. |
| `sshfs username@host:/remote/dir /local/mount/point` | Mounts a remote filesystem over SSH (requires FUSE). |
| `sshfs -o IdentityFile=/path/to/key username@host:/remote/dir /local/mount/point` | Mounts remote filesystem using a specific SSH key. |
| `eval $(ssh-agent)` | Starts the SSH agent to manage keys. |
| `ssh-add /path/to/private/key` | Adds a private key to the SSH agent. |
| `ssh-keyscan hostname_or_ip >> ~/.ssh/known_hosts` | Retrieves the public key of a remote host. |
| `ssh-keyscan -p port host >> ~/.ssh/known_hosts` | Retrieves a remote host's public key from a specific port. |
| `ssh username@host 'command'` | Executes a command on a remote host without starting an interactive shell. |
| `ssh username@host 'command' > local_output.txt` | Executes a command remotely and saves output locally. |
| `ssh username@host 'command' < local_input.txt` | Sends local file content as input to a remote command. |
| `ssh -i /path/to/private/key username@host` | Uses a custom identity file for SSH connection. |
| `ssh -p port username@host` | Connects to SSH on a non-standard port. |
| `ssh -L local_port:remote_host:remote_port username@host` | Local port forwarding (local → remote). |
| `ssh -R remote_port:target_host:target_port username@host` | Remote port forwarding (remote → another host). |
| `ssh -D local_port username@host` | Dynamic port forwarding (SOCKS proxy). |
| `ssh -D local_address:local_port username@host` | Dynamic port forwarding bound to a specific local address. |
| `ssh -X username@host` | Enables X11 forwarding for running remote GUI apps locally. |
| `ssh -L local_port:localhost:mysql_port username@host` | Tunnels a remote MySQL server to local. |
| `ssh -L 5901:localhost:5900 username@host` | Tunnels VNC over SSH for secure remote desktop access. |
| `ssh -J jump_user@jump_host:port target_user@target_host` | Uses a jump host (ProxyJump) to reach target server. |
| `rsync -avz -e ssh /local/path username@host:/remote/path` | Synchronizes files over SSH using `rsync`. |
| `pdsh -w user@host1,user@host2 'command'` | Runs a command on multiple servers simultaneously. |
| `ssh-keygen -t rsa -b 4096` | Generates a 4096-bit RSA SSH key. |
| `ssh-keygen -t rsa -C "comment"` | Generates an SSH key with a custom comment. |
| `command="/path/to/script" ssh-rsa AAAA...` | Restricts an SSH key to a specific command. |
| `sftp username@host` | Starts interactive SFTP session. |
| `sftp -b batch_commands.txt username@host` | Runs SFTP in batch mode for automation. |
| `scp user1@host1:/file user2@host2:/dest` | Copies a file directly between two remote servers. |
| `ssh -C username@host` | Enables compression for faster transfers over slow connections. |

---

## SSH Configuration File (`~/.ssh/config`)

The `~/.ssh/config` file allows you to define shortcuts and customize SSH behavior for different hosts.

### **Basic Syntax**
```ssh-config
Host <alias>
    HostName <real_hostname_or_ip>
    User <username>
    Port <port_number>
    IdentityFile <path_to_private_key>
```

**Example:**
```ssh-config
Host myserver
    HostName 192.168.1.10
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_rsa
```
Now you can connect with:
```bash
ssh myserver
```

---

### **Common `.ssh/config` Options**

| Option | Description |
|--------|-------------|
| `Host` | Alias name for the connection. Can also be a pattern (e.g., `*.example.com`). |
| `HostName` | Actual hostname or IP address of the server. |
| `User` | Default username for the connection. |
| `Port` | SSH port (default: 22). |
| `IdentityFile` | Path to the private key file. |
| `ProxyJump` | Jump/bastion host for connecting to another server. |
| `ForwardAgent` | Enables SSH agent forwarding (`yes`/`no`). |
| `Compression` | Enables compression (`yes`/`no`). |
| `ServerAliveInterval` | Time (in seconds) between keep-alive messages. |
| `ServerAliveCountMax` | Number of unanswered keep-alives before disconnecting. |
| `StrictHostKeyChecking` | Controls host key verification (`yes`, `no`, `ask`). |
| `UserKnownHostsFile` | Specifies file to store known hosts (default: `~/.ssh/known_hosts`). |
| `LocalForward` | Local port forwarding (`local_port remote_host:remote_port`). |
| `RemoteForward` | Remote port forwarding (`remote_port local_host:local_port`). |
| `DynamicForward` | Dynamic port forwarding (SOCKS proxy). |
| `ControlMaster` | Enables connection sharing (`yes`, `no`, `auto`). |
| `ControlPath` | Path to the control socket for connection sharing. |
| `ControlPersist` | Keeps the master connection open (`yes`, `no`, or time in seconds). |
| `LogLevel` | Verbosity of SSH logs (`QUIET`, `ERROR`, `INFO`, `VERBOSE`, etc.). |

---

### **Advanced Examples**

**1. Multiple Aliases for Same Server**
```ssh-config
Host web1 webserver prod-web
    HostName 203.0.113.10
    User ubuntu
    IdentityFile ~/.ssh/prod_key
```

**2. Using a Jump Host**
```ssh-config
Host target
    HostName 10.0.0.5
    User ec2-user
    ProxyJump bastion
```

**3. Per-Host Keep-Alive and Compression**
```ssh-config
Host database
    HostName db.example.com
    User dbadmin
    Compression yes
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

**4. Port Forwarding via Config**
```ssh-config
Host tunnel
    HostName mydb.example.com
    User admin
    LocalForward 3306 localhost:3306
```

**5. Wildcard Configurations**
```ssh-config
Host *.dev.example.com
    User developer
    IdentityFile ~/.ssh/dev_key
```

**6. Disable Host Key Checking (use with caution!)**
```ssh-config
Host testserver
    HostName 192.168.10.5
    User root
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
```

---

### **Tips**
- Permissions: `chmod 600 ~/.ssh/config` (required for security).
- Use comments (`#`) to document entries.
- You can have multiple `Host` blocks in the same file.
- Order matters: The first matching `Host` entry is used.
- Patterns like `?` and `*` are supported for multiple hosts.

---
