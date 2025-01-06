---
layout: page
title: My Unix Commands
permalink: /my_unix/
---

Nice to remember terminal stuff on the MacOS.
## SSH (Secure SHell)
# Connect to a remote server
```zsh
❯ ssh user@89.160.39.33
user@89.160.39.33's password:
Last login: Mon Jan  6 17:44:45 2025 from 89.160.39.33
[user@archlinux ~]$
```
When I connect to my linux-server

# Use a specific key for authentication
```zsh
ssh -i /path/to/private_key username@hostname  
```

# Run a single command on a remote server
```zsh
ssh username@hostname "ls -al /remote/path"  
```

## SCP (Secure Copy Protocol)
# Copy a file from local to remote
```zsh
scp local_file username@hostname:/remote/path  
```

# Copy a file from remote to local
```zsh
scp username@hostname:/remote/path local_file  
```

# Copy an entire directory (recursive)
```zsh
scp -r local_directory username@hostname:/remote/path  
```

## Mixed
```zsh
tomkarlsson@Toms-MBP ~ % shasum -a 256 filename
8de4bab1ab5424707f94f3a4f0690061fce9807d57f132245ca06da6fed96a5e  filename
```
Gets the 'Secure Hash Algorithms' Sum. Switch out 256 for different algorithms. 
