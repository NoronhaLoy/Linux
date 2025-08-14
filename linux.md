# Linux Shortcuts & Commands — DevOps/SRE Edition

## 1. Navigation & File Operations
| Command | Description | Notes |
|---------|-------------|-------|
| `pwd` | Show current directory path | Absolute path |
| `cd /path` | Change directory | Use `cd -` to go back |
| `ls -lah` | List files with permissions, size, hidden files | Add `--color=auto` for color |
| `tree` | Show directory tree | Needs `tree` package |
| `find . -name "*.log"` | Find files matching pattern | Add `-type f` for files only |
| `du -sh *` | Disk usage summary for each item in dir | Use to find large dirs |
| `df -h` | Disk space usage | Human-readable |
| `touch file.txt` | Create empty file | Updates timestamp if exists |
| `mkdir -p /path/subdir` | Create directory with parents | Avoids errors |
| `rm -rf /path` | Delete folder recursively & force | **Dangerous — double check path** |
| `cp -r src/ dest/` | Copy directory recursively | `-p` preserves timestamps |
| `mv file1 file2` | Move or rename file | Works for dirs too |
| `rsync -avh src/ dest/` | Sync directories | `--delete` to remove extras |

---

## 2. Process & Job Control
| Command | Description | Notes |
|---------|-------------|-------|
| `ps aux` | Show all processes | Combine with `grep` to filter |
| `top` | Real-time process monitor | Press `q` to quit |
| `htop` | Better process monitor | Needs `htop` installed |
| `pgrep nginx` | Find process by name | Add `-a` for full command |
| `pkill -f pattern` | Kill processes matching pattern | Example: `pkill -f gunicorn` |
| `kill -9 <pid>` | Force kill process | Use sparingly |
| `jobs` | Show background jobs | Works after `Ctrl+Z` |
| `fg %1` | Bring job to foreground | `%<job_id>` |
| `bg %1` | Resume job in background | Useful for paused jobs |
| `nohup cmd &` | Run command immune to hangups | Output in `nohup.out` |
| `screen -S name` | Start screen session | `screen -r name` to attach |
| `tmux` | Persistent terminal multiplexer | Split panes |

---

## 3. Networking
| Command | Description | Notes |
|---------|-------------|-------|
| `ip addr` | Show IP addresses | Replaces `ifconfig` |
| `ip route` | Show routing table | Useful for debugging |
| `ping host` | Test connectivity | `-c 4` limits packets |
| `traceroute host` | Show route to host | Needs `traceroute` package |
| `curl -I https://example.com` | Fetch HTTP headers | `-L` follows redirects |
| `wget url` | Download file from URL | Use `-O` for filename |
| `netstat -tulpn` | Show open ports & listening services | Needs `net-tools` |
| `ss -tulpn` | Modern netstat replacement | Faster output |
| `nc -zv host port` | Check if port open | Good for debugging |
| `dig domain` | DNS lookup | Add `+short` for just IP |
| `nslookup domain` | DNS lookup (legacy) | Installed by default in some distros |
| `whois domain` | Domain registry info | Needs `whois` package |

---

## 4. Logs & Text Processing
| Command | Description | Notes |
|---------|-------------|-------|
| `tail -f file.log` | Follow log file in real-time | `-n 100` for last 100 lines |
| `less file.log` | View file with scroll | `/pattern` to search |
| `grep -i "error" file.log` | Case-insensitive search | Add `-r` for recursive |
| `grep -r "keyword" /path` | Search inside files recursively | |
| `cut -d':' -f1 file.txt` | Split and select field by delimiter | |
| `awk '{print $1,$3}' file.txt` | Extract fields from text | Space-separated by default |
| `sort file.txt` | Sort lines | `-n` numeric |
| `uniq -c` | Count duplicates | Often used with `sort` |
| `wc -l file.txt` | Count lines | `-w` for words |
| `head -n 20 file.txt` | First 20 lines | |
| `diff file1 file2` | Compare files | Use `-u` for unified diff |

---

## 5. Compression & Archives
| Command | Description | Notes |
|---------|-------------|-------|
| `tar -czvf file.tar.gz /path` | Create tar.gz archive | `-x` to extract |
| `zip -r file.zip /path` | Create zip file | `unzip file.zip` to extract |
| `gzip file` | Compress file | `gunzip file.gz` to decompress |
| `xz file` | Compress with xz | `unxz file.xz` to decompress |

---

## 6. Permissions & Ownership
| Command | Description | Notes |
|---------|-------------|-------|
| `chmod 755 file` | Change permissions | `r=4, w=2, x=1` sum for each role |
| `chmod -R 644 dir` | Recursive permission change | |
| `chown user:group file` | Change owner & group | Needs `sudo` if not owner |
| `ls -l` | Show file permissions | First column shows mode |

---

## 7. Monitoring & System Info
| Command | Description | Notes |
|---------|-------------|-------|
| `uptime` | Show system uptime | Includes load average |
| `uptime -p` | Pretty uptime format | |
| `free -h` | Memory usage | Human-readable |
| `vmstat 1` | CPU/memory stats every sec | |
| `iostat` | Disk I/O stats | Needs `sysstat` |
| `sar -u 1 5` | CPU usage for 5 intervals | Needs `sysstat` |
| `df -i` | Inode usage | Troubleshoot "disk full" due to inodes |
| `lsof -i :80` | Show processes using port 80 | Needs `lsof` |

---
