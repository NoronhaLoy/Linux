# Comprehensive Shell Scripting Guide for SRE / DevOps / Platform / Cloud Engineers

## 1. Basics

* Shebang: `#!/bin/bash`
* Execute: `bash script.sh` or `chmod +x script.sh && ./script.sh`
* Comments: `# This is a comment`
* Variables: `VAR=value`, access with `$VAR`
* Environment variables: `export VAR=value`
* Positional parameters: `$1, $2, $@, $*`
* Special variables:

  * `$0` : Name of the script
  * `$1, $2, ...` : Positional parameters
  * `$#` : Number of positional parameters
  * `$*` : All parameters as a single word
  * `$@` : All parameters as separate words
  * `$?` : Exit status of last command
  * `$$` : PID of the script
  * `$!` : PID of last background process
  * `$-` : Current options set for the shell
  * `$IFS` : Internal Field Separator
  * `$OLDPWD` : Previous working directory
  * `$PWD` : Current working directory
  * `$_` : Last argument of previous command
  * `$REPLY` : Default variable used by `read` when no variable is specified

**Example:**

```bash
#!/bin/bash
NAME="Server"
echo "Hello $NAME"
echo "Script PID: $$"
echo "Total args: $#"
```

---

## 2. Operators

* Arithmetic: `+ - * / %` or `((...))`
* Comparison: `-eq -ne -lt -le -gt -ge`
* String: `= != < >` (lexical)
* Logical: `&& || !`
* File tests: `-f -d -r -w -x -s`

**Example:**

```bash
NUM=5
if [ $NUM -gt 3 ]; then
    echo "Number is greater than 3"
fi
```

---

## 3. Conditionals

### If-Else

```bash
if [ -f /tmp/file.txt ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

### Elif

```bash
if [ -f file1 ]; then
    echo "File1 exists"
elif [ -f file2 ]; then
    echo "File2 exists"
else
    echo "No file found"
fi
```

### Case Statement

```bash
case $1 in
    start) echo "Starting service" ;;
    stop) echo "Stopping service" ;;
    restart) echo "Restarting service" ;;
    *) echo "Unknown command" ;;
esac
```

---

## 4. Loops

### For Loop

```bash
for i in {1..5}; do
    echo "Iteration $i"
done
```

### For Loop with Array

```bash
ARR=(apple banana cherry)
for FRUIT in "${ARR[@]}"; do
    echo "Fruit: $FRUIT"
done
```

### While Loop

```bash
COUNT=1
while [ $COUNT -le 5 ]; do
    echo "Count: $COUNT"
    ((COUNT++))
done
```

### Until Loop

```bash
COUNT=1
until [ $COUNT -gt 5 ]; do
    echo "Count: $COUNT"
    ((COUNT++))
done
```

---

## 5. Functions

```bash
function greet() {
    echo "Hello $1"
}
greet "World"
```

* Return values: `return 0` or `echo value`

---

## 6. Arrays

```bash
ARR=(one two three)
echo ${ARR[0]}       # Access first element
echo ${ARR[@]}      # All elements
echo ${#ARR[@]}     # Length of array
```

---

## 7. String Manipulation

```bash
STR="Hello World"
echo ${STR/World/Everyone}   # Replace first occurrence
echo ${STR//o/O}             # Replace all occurrences
echo ${STR:6:5}              # Substring
echo ${#STR}                 # Length
```

---

## 8. Input/Output

```bash
read -p "Enter your name: " NAME
echo "Hello $NAME"
```

Redirection and pipes:

```bash
ls > output.txt
cat file.txt | grep "error"
```

Here document:

```bash
cat <<EOF > file.txt
Line 1
Line 2
EOF
```

---

## 9. Command-line Parsing

```bash
while getopts ":u:p:h" opt; do
    case $opt in
        u) USER=$OPTARG ;;
        p) PASS=$OPTARG ;;
        h) echo "Usage: $0 -u user -p pass" ; exit 0 ;;
        *) echo "Invalid option" ; exit 1 ;;
    esac
done
```

---

## 10. Process & Job Management

```bash
ps aux | grep myprocess
kill -9 <PID>
jobs
fg %1
bg %1
nohup myscript.sh &
```

---

## 11. Error Handling & Debugging

```bash
if command; then
    echo "Success"
else
    echo "Failure"
fi
```

```bash
set -euxo pipefail  # Debug, fail on error, show commands
```

---

## 12. Logging

```bash
LOG_FILE="/tmp/script.log"
echo "$(date) - Script started" >> $LOG_FILE
```

---

## 13. Automation with Cron

```bash
0 2 * * * /home/user/backup.sh
```

* Use `crontab -e` to edit
* Redirect stdout/stderr to log files

---

## 14. Networking Commands

Ping and curl examples:

```bash
if ping -c 1 google.com; then echo "Network OK"; fi
STATUS=$(curl -o /dev/null -s -w "%{http_code}" https://example.com)
```

Port checks:

```bash
nc -zv host port
```

---

## 15. Advanced Topics

* Trap signals: `trap 'echo Interrupted; exit' SIGINT`
* Process substitution: `diff <(ls dir1) <(ls dir2)`
* Parallel execution: `&` and `wait`
* Conditional expressions: `[[ ... ]]` for regex and advanced tests

---

## 16. Best Practices

* Quote variables
* Validate input
* Comment code
* Logging and monitoring
* Cleanup temporary files
* Use functions for reusable logic
* Return proper exit codes

---

## 17. Real-World Examples

### Automated Backups

```bash
BACKUP_DIR="/backup/$(date +%F)"
mkdir -p "$BACKUP_DIR"
rsync -av /data/ "$BACKUP_DIR"
echo "Backup complete: $(date)" >> /var/log/backup.log
```

### Health Check Script

```bash
SERVICES=(nginx mysql redis)
for SVC in "${SERVICES[@]}"; do
    if systemctl is-active --quiet $SVC; then echo "$SVC OK"; else echo "$SVC FAILED"; fi
done
```

### Deployment Rollback

```bash
CURRENT_VERSION=$(cat /var/app/version)
PREV_VERSION=$(cat /var/app/version.bak)
cp -r /var/app/releases/$PREV_VERSION/* /var/app/
systemctl restart myapp
```

### Log Rotation Example

```bash
LOG_DIR="/var/log/myapp"
find $LOG_DIR -type f -name "*.log" -mtime +7 -exec gzip {} \;
```

### Service Monitoring

```bash
if ! pgrep -x "nginx" > /dev/null; then
    systemctl start nginx
fi
```

### Backup with Error Handling

```bash
mkdir -p /backup || { echo "Failed to create backup dir"; exit 1; }
rsync -av /data/ /backup || echo "Backup failed"
```

### Deployment Notifications

```bash
STATUS=$(systemctl is-active myapp)
if [ "$STATUS" != "active" ]; then
    echo "myapp is down!" | mail -s "Alert" admin@example.com
fi
```

---

This guide now includes complete shell scripting essentials, all special variables, advanced techniques, and real-world examples for SRE, DevOps, Platform, and Cloud Engineers.
