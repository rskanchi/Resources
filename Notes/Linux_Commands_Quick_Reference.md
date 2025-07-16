# Quick-reference Linux Commands

## File and Directory Management

### Listing Files and Directories
- `ls`: List files and directories in the current location.
- `ls -l`: Long listing format with file details.
- `ls -a`: Show hidden files.
- `ls -lh`: Long listing format with human-readable file sizes.

### symbolik links
- `ln -s /path/to/original /path/to/link`: Create a symbolic link to a file or directory.  
- `ls -l /path/to/link`: Show details of the symbolic link like where the synlink points.   
- `ln -sfn /new/target /path/to/link`: Update the target of an existing symbolic link.  
- `rm /path/to/link`: Remove the symbolic link without deleting the original file.   
- `find . -type l`: Find all symbolic links in the current directory and its subdirectories.   
- `find . -type l -name "*fastq.gz" -exec ls -l {} +`: Find and list all symbolic links with the name pattern "*fastq.gz" in the current directory and its subdirectories.  
- `find . -type l -name "*fastq.gz" -exec rm {} +`: Find and delete all symbolic links with the name pattern "*fastq.gz" in the current directory and its subdirectories.  


### Navigating Directories
- `cd /path/to/directory`: Change to a specific directory.
- `cd ..`: Move up one directory.
- `cd ~`: Go to the home directory.
- `cd -`: Switch to the previous directory.

### Creating and Deleting
- `mkdir folder_name`: Create a new folder.
- `rmdir folder_name`: Delete an empty folder.
- `rm -r folder_name`: Delete a folder and its contents.
- `touch file_name`: Create an empty file or update the timestamp.
- `rm file_name`: Delete a file.

### Copying and Moving
- `cp source destination`: Copy a file.
- `cp -r source_folder destination`: Copy a folder and its contents.
- `mv source destination`: Move or rename a file or folder.
- `rsync -avh <source_folder/file_pattern> <username@cluster:/target/folder/path>`: copy files from local to cluster folder. 

### Viewing File Contents
- `cat file_name`: Display file contents.
- `less file_name`: View file contents page by page.
- `head file_name`: Show the first 10 lines of a file.
- `tail file_name`: Show the last 10 lines of a file.
- `tail -f file_name`: Continuously monitor a file for new lines (useful for logs).

---

## Permissions and Ownership

### Checking Permissions
- `ls -l`: Displays file permissions in the listing.

### Changing Permissions
- `chmod 755 file_name`: Set permissions (read/write/execute for owner, read/execute for others).
- `chmod +x file_name`: Make a file executable.

---

## Searching

### Finding Files and Directories
- `find /path -name "file_name"`: Search for a file by name.
- `find /path -type d -name "folder_name"`: Search for a folder by name.

### Searching in Files
- `grep "search_term" file_name`: Search for a term in a file.
- `grep -r "search_term" folder_name`: Search for a term recursively in a folder.
- `grep -i "search_term" file_name`: Case-insensitive search.

---

## Process Management

### Viewing Processes
- `ps`: Show running processes for the current session.
- `ps aux`: Show all running processes.
- `top`: Display active processes in real-time.
- `htop`: Interactive process viewer (requires installation).

### Managing Processes
- `kill PID`: Terminate a process by its ID.
- `killall process_name`: Kill all processes by name.
- `ctrl + c`: Stop the current process in the terminal.

---

## System Information

### General Info
- `uname -a`: Display system information.
- `df -h`: Show disk space usage.
- `du -sh folder_name`: Show the size of a folder.
- `free -h`: Display memory usage.
- `uptime`: Show system uptime and load averages.

---

## Archiving and Compression

### Creating and Extracting Archives
- `tar -cvf archive.tar folder_name`: Create a tar archive.
- `tar -xvf archive.tar`: Extract a tar archive.
- `tar -czvf archive.tar.gz folder_name`: Create a compressed tar archive.
- `tar -xzvf archive.tar.gz`: Extract a compressed tar archive.

### Compressing and Decompressing
- `gzip file_name`: Compress a file.
- `gunzip file_name.gz`: Decompress a gzip file.
- `zip archive.zip file_name`: Compress a file into a zip archive.
- `unzip archive.zip`: Extract a zip archive.

---

## Miscellaneous

### File Permissions and Ownership
- `chmod 755 file_name`: Change file permissions.
- `chown user:group file_name`: Change file ownership.

### Viewing Logs
- `dmesg`: View kernel logs.
- `journalctl`: View system logs (use `-xe` for detailed errors).

### Checking Network Information
- `ping domain.com`: Test connectivity to a domain.
- `curl http://example.com`: Fetch a webpage or file from a URL.
- `wget http://example.com`: Download a file from a URL.
- `ifconfig`: Display network interface information (use `ip a` in newer systems).

### Terminal Navigation Shortcuts
- `ctrl + a`: Move to the beginning of the line.
- `ctrl + e`: Move to the end of the line.
- `ctrl + r`: Search command history.
- `ctrl + l`: Clear the terminal screen.
- `ctrl + c`: Interrupt the current process.

---