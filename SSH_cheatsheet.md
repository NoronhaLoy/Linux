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
