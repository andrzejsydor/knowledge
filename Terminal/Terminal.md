---
tags:
  - Tools
---

# Terminal

## Tools

### Terminal Emulators

[iTerm2](https://iterm2.com/) [https://github.com/gnachman/iTerm2](https://github.com/gnachman/iTerm2) - macOS terminal replacement

[Warp - The intelligent terminal](https://www.warp.dev/) - Modern terminal with AI features

### Shell Enhancements

[ohmyz - Unleash your terminal like never before](https://ohmyz.sh/) - Zsh framework

[Starship - Cross-Shell prompt](https://starship.rs/guide/) - Fast, customizable prompt

### Editors

[Cursor - The AI Code Editor](https://www.cursor.com/) - AI-powered code editor

---

## Essential Commands

### Navigation

```bash
# Change directory
cd /path/to/directory
cd ~                    # Home directory
cd -                    # Previous directory
cd ..                   # Parent directory

# List files
ls                      # List files
ls -la                  # List all files with details
ls -lh                  # Human-readable file sizes
ls -lt                  # Sort by modification time
ls -R                   # Recursive listing

# Print working directory
pwd                     # Current directory path
```

### File Operations

```bash
# Create files and directories
touch file.txt          # Create empty file
mkdir dirname           # Create directory
mkdir -p path/to/dir    # Create nested directories

# Copy, move, remove
cp file.txt backup.txt  # Copy file
cp -r dir/ backup/      # Copy directory recursively
mv file.txt newname.txt # Move/rename
rm file.txt             # Remove file
rm -rf dir/             # Remove directory recursively (dangerous!)

# View files
cat file.txt            # Print entire file
less file.txt           # View file with pagination
head -n 20 file.txt     # First 20 lines
tail -n 20 file.txt     # Last 20 lines
tail -f file.txt        # Follow file (watch for updates)
```

### File Permissions

```bash
# Change permissions
chmod 755 file.sh       # rwxr-xr-x
chmod +x file.sh        # Add execute permission
chmod -R 644 dir/       # Recursive permission change

# Change ownership
chown user:group file   # Change owner and group
chown -R user:group dir/ # Recursive ownership change

# View permissions
ls -l                   # Long format with permissions
stat file.txt           # Detailed file information
```

### Text Processing

```bash
# Search and replace
grep "pattern" file.txt           # Search for pattern
grep -r "pattern" dir/            # Recursive search
grep -i "pattern" file.txt        # Case-insensitive
grep -v "pattern" file.txt        # Invert match (exclude)

# Advanced text processing
sed 's/old/new/g' file.txt        # Replace text
awk '{print $1}' file.txt         # Print first column
cut -d',' -f1 file.csv            # Cut by delimiter

# Sort and unique
sort file.txt                     # Sort lines
sort -u file.txt                  # Sort and remove duplicates
uniq file.txt                     # Remove consecutive duplicates
```

### Process Management

```bash
# View processes
ps aux                            # All processes
ps aux | grep process_name        # Filter processes
top                               # Interactive process viewer
htop                              # Enhanced top (if installed)

# Kill processes
kill PID                          # Kill by process ID
kill -9 PID                       # Force kill
killall process_name              # Kill all processes by name
pkill -f pattern                  # Kill by pattern

# Background jobs
command &                         # Run in background
jobs                              # List background jobs
fg %1                             # Bring job to foreground
bg %1                             # Resume background job
nohup command &                   # Run command immune to hangups
```

### System Information

```bash
# System info
uname -a                          # System information
whoami                            # Current user
id                                # User and group IDs
uptime                            # System uptime
df -h                             # Disk space usage
du -sh dir/                       # Directory size
free -h                           # Memory usage (Linux)
vm_stat                           # Memory stats (macOS)

# Network
ifconfig                          # Network interfaces
ip addr                           # IP addresses (Linux)
netstat -an                       # Network connections
lsof -i :8080                     # Processes using port 8080
ping hostname                     # Ping host
curl url                          # HTTP request
wget url                          # Download file
```

### History and Aliases

```bash
# Command history
history                           # View command history
history | grep pattern            # Search history
!!                                # Repeat last command
!n                                # Execute command number n
!string                           # Execute last command starting with string
Ctrl+R                            # Search history interactively

# Aliases (add to ~/.zshrc or ~/.bashrc)
alias ll='ls -lah'
alias la='ls -A'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias f='find . -name'
alias h='history'
alias c='clear'
```

### Redirection and Pipes

```bash
# Output redirection
command > file.txt                # Overwrite file
command >> file.txt               # Append to file
command 2> error.log              # Redirect stderr
command > file.txt 2>&1           # Redirect both stdout and stderr
command &> file.txt               # Redirect both (bash/zsh)

# Input redirection
command < file.txt                # Read from file
command << EOF                     # Here document
text
EOF

# Pipes
command1 | command2               # Pipe output
command1 | grep pattern | sort    # Chain pipes
```

### Compression and Archives

```bash
# tar
tar -czf archive.tar.gz dir/      # Create compressed archive
tar -xzf archive.tar.gz           # Extract archive
tar -tzf archive.tar.gz           # List archive contents

# zip
zip -r archive.zip dir/           # Create zip archive
unzip archive.zip                 # Extract zip
unzip -l archive.zip              # List zip contents

# Other
gzip file.txt                     # Compress file
gunzip file.txt.gz                # Decompress
```

### Find and Locate

```bash
# find
find . -name "*.txt"              # Find files by name
find . -type f -name "*.txt"      # Find files only
find . -type d -name "dir"        # Find directories
find . -size +100M                # Find files larger than 100MB
find . -mtime -7                  # Modified in last 7 days
find . -exec rm {} \;             # Execute command on results

# locate (faster, uses database)
locate filename                   # Find file
updatedb                          # Update locate database (Linux)
sudo /usr/libexec/locate.updatedb # Update on macOS
```

### Environment Variables

```bash
# View and set
echo $PATH                        # View PATH variable
export VAR=value                  # Set environment variable
env                               # List all environment variables
printenv                          # Print environment variables

# Common variables
$HOME                             # Home directory
$USER                             # Current user
$PWD                              # Current directory
$SHELL                            # Current shell
```

### Shortcuts and Tips

```bash
# Keyboard shortcuts
Ctrl+C                            # Cancel current command
Ctrl+D                            # Exit shell / EOF
Ctrl+L                            # Clear screen
Ctrl+A                            # Move to beginning of line
Ctrl+E                            # Move to end of line
Ctrl+U                            # Clear line before cursor
Ctrl+K                            # Clear line after cursor
Ctrl+W                            # Delete word before cursor
Ctrl+R                            # Search history
Tab                               # Auto-complete
Tab Tab                           # Show completion options

# Useful combinations
cd !$                             # cd to last argument of previous command
^old^new                          # Replace 'old' with 'new' in last command
```

### macOS Specific

```bash
# System commands
open file.txt                     # Open file with default app
open -a "App Name" file.txt       # Open with specific app
open .                            # Open current directory in Finder
pbcopy < file.txt                 # Copy file to clipboard
pbpaste                           # Paste from clipboard
defaults read                     # Read system defaults
brew install package              # Homebrew package manager

# Process management
launchctl list                    # List launch agents
launchctl load ~/Library/LaunchAgents/  # Load agent
```

### Advanced Techniques

```bash
# Command substitution
$(command)                        # Execute command and use output
`command`                         # Backticks (older syntax)
ls $(date +%Y)                    # Use date output in command

# Brace expansion
mkdir {dir1,dir2,dir3}            # Create multiple directories
cp file.{txt,bak}                 # Copy file.txt to file.bak
echo {1..10}                      # Print 1 to 10

# Conditional execution
command1 && command2              # Run command2 if command1 succeeds
command1 || command2              # Run command2 if command1 fails
command1 ; command2               # Run both sequentially

# Background and foreground
(command1 && command2) &          # Run in background
disown                            # Disown background job
```

### Useful One-Liners

```bash
# Find and replace in multiple files
find . -type f -name "*.txt" -exec sed -i 's/old/new/g' {} +

# Count lines of code
find . -name "*.py" | xargs wc -l

# Find largest files
find . -type f -exec ls -lh {} \; | sort -k5 -hr | head -10

# Monitor log file
tail -f /var/log/system.log | grep error

# Kill all processes matching pattern
ps aux | grep pattern | awk '{print $2}' | xargs kill

# Create backup with timestamp
cp file.txt file.txt.$(date +%Y%m%d_%H%M%S)
```

### Shell Scripting Basics

```bash
#!/bin/bash                       # Shebang for bash
#!/bin/zsh                        # Shebang for zsh

# Variables
VAR="value"
echo $VAR

# Conditionals
if [ condition ]; then
    command
elif [ condition ]; then
    command
else
    command
fi

# Loops
for i in {1..10}; do
    echo $i
done

while [ condition ]; do
    command
done

# Functions
function_name() {
    local var="local"
    echo "Function body"
}
```

### Best Practices

1. **Use quotes** for variables with spaces: `"$VAR"`
2. **Check if file exists**: `[ -f file.txt ] && command`
3. **Use `set -e`** in scripts to exit on error
4. **Use `set -u`** to exit on undefined variables
5. **Always quote variables** to prevent word splitting
6. **Use `[[ ]]` instead of `[ ]`** in bash/zsh for better features
7. **Prefer `$(command)` over backticks** for command substitution
8. **Use meaningful variable names** in scripts
9. **Add comments** to complex commands
10. **Test scripts** before using in production

---

## Resources

- [Bash Guide](https://mywiki.wooledge.org/BashGuide)
- [Zsh Documentation](https://zsh.sourceforge.io/Doc/)
- [Command Line Power User](https://commandlinepoweruser.com/)
