# The Terminal

The terminal in Linux is an interface in which you can type and execute text-based commands. It allows for efficient management of the operating system and software, providing a direct way to interact with the system's kernel and services. Unlike graphical user interfaces (GUIs), the terminal provides a lightweight, more controlled, and scriptable way of interacting with the computer.

## Accessing the Terminal

**Graphical Method**: On most Linux desktop environments, you can open the terminal from the applications menu. Look for "Terminal".

**Keyboard Shortcut**: Many distributions allow you to open a terminal window by pressing `Ctrl + Alt + T`.

## File Management Commands

### Basic Commands

#### `pwd`

Prints the path of the current working directory.

#### `cd`

Changes the working directory, abbreviation for 'change directory'.
- `cd <folder>`: Go into a folder.
- `cd`: Go to home directory.
- `cd ..`: Go 1 directory up.
- `cd ../..`: Go 2 directories up.
- `cd ../../etc`: Go 2 directories up and go to the `etc` folder.
- `cd ~`: Go to your home directory.

#### `mkdir`

Creates a folder inside the working directory.
- `mkdir dir1`: Create a folder named `dir1` in the current directory.
- `mkdir dir1 dir2 dir3`: Create multiple folders in the current directory.
- `mkdir ~/Desktop/dir1`: Create a folder named `dir1` on the existing desktop directory.
- `mkdir -p dir1/dir2/dir3`: Create parented directories, creating intermediate folders if they don't exist.

#### `ls`

Lists files in the working directory.
- `ls`: List files and folders in the current directory.
- `ls -a`: List all files, including hidden ones.
- `ls -l`: Detailed list view with file information such as permissions and size.
- `ls -lh`: List files in a human-readable format.
- `ls dir1`: List the contents of the `dir1` directory.

#### `touch`

Creates a new empty file.
- `touch output.txt`: Creates an empty file named `output.txt`.

#### `>` and `>>`

Writes output to files:
- `>`: Writes the output of a command to a file, overwriting the file if it exists.
  - `pwd > output.txt`: Writes the path of the working directory to `output.txt`.
- `>>`: Appends the output of a command to a file.
  - `pwd >> output.txt`: Appends the path of the working directory to `output.txt`.

### Viewing and Manipulating File Content

#### `cat`

Stands for Concatenate. Creates, views, and concatenates files:
- `cat output.txt`: Displays the contents of `output.txt`.
- `cat file1.txt file2.txt > merged.txt`: Concatenates two files into `merged.txt`.

#### `mv`

Moves or renames files and directories:
- `mv output.txt dir1/`: Moves `output.txt` to the `dir1` directory.
- `mv dir1/* .`: Moves everything from `dir1` to the current directory.
- `mv oldname.txt newname.txt`: Renames a file.

#### `cp`

Copies files and directories:
- `cp output.txt output2.txt`: Copies `output.txt` to `output2.txt`.
- `cp -r dir1 dir2`: Recursively copies the `dir1` directory to `dir2`.

#### `rm`

Command that permanently deletes files and directories. There is no "trash" or "recycle bin" by default. Once you delete something with rm, it's usually gone for good.
- `rm myfile.txt`: Removes 1 file.
- `rm file1.txt file2.txt file3.txt`: Removes multiple files.
- `rm *.txt`: Removes files using wildcards.
- `rm -d my_empty_directory/`: Removes an empty directory.
- `rm -r my_directory/`: Removes a directory and its contents (recursively).
- `rm -f file.txt`: Force deletion.

#### `trash-cli`

Command-line interface for the "Trash" or "Recycle Bin" functionality that you typically find in graphical desktop environments (like GNOME).
This utility moves files and directories to the Trash: Instead of permanently deleting them, you send them to a temporary holding area from which they can be recovered.

- `trash-put myfile.txt`: Trash a single file
- `trash-put file1.txt file2.txt`: Trash multiple files
- `trash-put my_directory/`: Trash a directory (and its contents)
- `trash-put *.txt`: Trash all .txt files in the current directory
- `trash my_directory`: Trash a directory


## File Management Commands

### Basic Commands

#### `pwd`

Prints the path of the current working directory: `pwd`

#### `cd`

Changes the working directory:
- `cd`: Go to the home directory.
- `cd ..`: Go one directory up.
- `cd ../..`: Go two directories up.
- `cd ../../etc`: Go two directories up and into the `etc` folder.
- `cd ~`: Go to the user's home directory.
- `cd -`: Switch back to the previous working directory.

#### `mkdir`

Creates a folder inside the working directory:
- `mkdir dir1`: Create a folder named `dir1` in the current directory.
- `mkdir dir1 dir2 dir3`: Create multiple folders in the current directory.
- `mkdir ~/Desktop/dir1`: Create a folder named `dir1` on the desktop.
- `mkdir -p dir1/dir2/dir3`: Create parented directories, creating intermediate folders if they don't exist.

#### `ls`

Lists files in the working directory:
- `ls`: List files and folders in the current directory.
- `ls -a`: List all files, including hidden ones.
- `ls -A`: List all files, including hidden ones but without the current and parent directories (`.` and `..`)
- `ls -l`: Detailed list view with file information such as permissions and size.
- `ls -lh`: List files in a human-readable format.
- `ls dir1`: List the contents of the `dir1` directory.

#### `touch`

Creates a new empty file:
- `touch output.txt`: Creates an empty file named `output.txt`.

#### `>` and `>>`

Writes output to files:
- `>`: Writes the output of a command to a file, overwriting the file if it exists.
  - Example: `pwd > output.txt`: Writes the path of the working directory to `output.txt`.
- `>>`: Appends the output of a command to a file.
  - Example: `pwd >> output.txt`: Appends the path of the working directory to `output.txt`.

### Viewing and Manipulating File Content

#### `cat`

Creates, views, and concatenates files:
- `cat output.txt`: Displays the contents of `output.txt`.
- `cat file1.txt file2.txt > merged.txt`: Concatenates two files into `merged.txt`.

#### `mv`

Moves or renames files and directories:
- `mv output.txt dir1/`: Moves `output.txt` to the `dir1` directory.
- `mv dir1/* .`: Moves everything from `dir1` to the current directory.
- `mv oldname.txt newname.txt`: Renames a file.

#### `cp`

Copies files and directories:
- `cp output.txt output2.txt`: Copies `output.txt` to `output2.txt`.
- `cp -r dir1 dir2`: Recursively copies the `dir1` directory to `dir2`.

#### `rm`

Removes files and directories:
- `rm output.txt`: Removes `output.txt`.
- `rm -rf dir1`: Recursively removes `dir1` and its contents.

#### `file`

Determines the file type of a given file:
- `file image.jpg`: Displays the file type of `image.jpg`.

#### `stat`: 

Displays detailed information about a file or file system:
- `stat filename.txt`: Shows file details including inode, size, and permissions.

#### `ln` 

Creates links between files.
- `ln -s source.txt link.txt`: Creates a symbolic link named `link.txt` pointing to `source.txt`.
- `ln source.txt link.txt`: Creates a hard link named `link.txt` to `source.txt`.

#### `wc`

Counts lines, words, and characters in a file:
- `wc filename.txt`: Displays line, word, and character count for the file.
- `wc -l filename.txt`: Displays the line count only.
- `ls -A | wc -l`: Displays the file count in the current directory.

#### `sed`

Stream editor used for filtering and transforming text:
- `sed 's/old/new/' file.txt`: Replaces the first occurrence of "old" with "new" in each line.
- `sed -i 's/old/new/g' file.txt`: Replaces all occurrences of "old" with "new" in the file.

Read more about `sed` [here](https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/).

#### `awk`

A powerful text processing tool:
- `awk '{print $1}' file.txt`: Prints the first column of a file.
- `awk '/pattern/ {print $0}' file.txt`: Prints lines matching a pattern.

Read more about `awk` [here](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/).

#### `cut`

Cuts specific sections from a file:
- `cut -d',' -f2 file.csv`: Extracts the second column from a CSV file using `,` as a delimiter.

### Viewing Large Files and Differences

#### `less`

Displays large files one page at a time:
- `less largefile.txt`: Opens `largefile.txt` for paginated viewing.

#### `head`

Displays the first few lines of a file:
- `head -n 5 file.txt`: Displays the first five lines of the file.

#### `tail`

Displays the last few lines of a file:
- `tail -n 5 file.txt`: Displays the last five lines of the file.
- `tail -f logfile.txt`: Continuously displays new lines added to a file in real time.

#### `diff`

Compares the contents of two files line by line:
- `diff file1.txt file2.txt`: Shows the differences line by line.

#### `sort`

Sorts the lines in a file:
- `sort file.txt`: Alphabetically sorts the file.
- `sort -n file.txt`: Sorts the file numerically.
- `sort -r file.txt`: Sorts the file in reverse order.
- `sort -k 2 file.txt`: Sorts by the second column.

### Editing and Compression

#### `nano`: 

Opens and edits files:
- `nano output.txt`: Opens `output.txt` for editing.

Read more about `nano` [here](https://www.geeksforgeeks.org/nano-text-editor-in-linux/).

#### `tar`

Creates, extracts, or manipulates archive files:
- `tar -cvf archive.tar file1 file2`: Creates an archive containing `file1` and `file2`.
- `tar -xvf archive.tar`: Extracts the contents of `archive.tar`.
- `tar -czvf archive.tar.gz dir/`: Creates a compressed archive of the `dir` directory.

#### `zip` and `unzip`

Compresses and extracts files:
- `zip myzip.zip file1 file2`: Compresses `file1` and `file2` into `myzip.zip`.
- `unzip myzip.zip`: Extracts the contents of `myzip.zip`.

### Organizing and Finding Files

#### `tree`

Displays files in a tree structure:
- `tree`: Displays the directory structure starting from the current directory.
- `tree -L 1`: Displays the directory structure up to a depth of 1.

#### `find`

Searches for files and directories based on various criteria:
- `find . -name "*.py"`: Finds all `.py` files in the current directory and subdirectories.
- `find . -iname "*.py"`: Performs a case-insensitive search for `.py` files.
- `find /home/user/projects -type d -name build`: Finds all directories named `build` in `/home/user/projects`.
- `find /home/user -mtime -7`: Finds files modified in the last 7 days in `/home/user`.
- `find /var/log -size +100M`: Finds files larger than 100MB in `/var/log`.
- `find /home/user/images -name "*.jpg" -exec mv {} /home/user/backup \;`: Finds all `.jpg` files in `/home/user/images` and moves them to `/home/user/backup`.
- `find /path/to/dir -name "*.tmp" -exec rm {} \;`: Finds all `.tmp` files and deletes them.

#### `grep`

Stands for “global regular expression print”, a powerful command used for searching text using patterns. 
- `apt list | grep firefox`: Filters the list of packages for "firefox."
- `grep "word" filename.txt`: Searches for a specific word in a file.
- `grep -r "word" /path/to/directory`: Recursively searches for a word in all files in a directory.
- `grep -A 3 "pattern" filename.txt`: Displays 3 lines after each pattern match.
- `grep -i "pattern" filename.txt`: Performs a case-insensitive search.
- `grep -c "pattern" filename.txt`: Counts the number of lines matching the pattern.

## General Commands

#### `whoami`

Outputs the username of the current user.

#### `echo`

Displays a string or text passed as an argument:
- `echo "Hello World"`: Displays "Hello World" in the terminal.

#### `clear`

Clears the terminal screen, making it blank.

#### `man`

Shows the manual page for commands, providing detailed information about command usage and options.
- `man mkdir`: Shows the manual for the `mkdir` command.

#### `info`

Provides a more detailed guide for commands:
- `info mkdir`: Shows detailed information about `mkdir`.

Displays previously entered commands in the terminal:
- `history`: Shows the full command history.
- `history | tail -n 20`: Displays the last 20 commands.
- `history -c`: Clears the command history.

## System Management Commands

### User Management

#### `chmod`

Changes file permissions:
- `chmod 755 file.txt`: Grants read, write, and execute permissions for the owner and read/execute for others.

#### `sudo`

Executes a command with superuser privileges:
- `sudo apt install gimp`: Installs the GIMP software.

#### `chown`

Changes file ownership:
- `chown user file.txt`: Assigns ownership of `file.txt` to `user`.

#### `chgrp`

Changes the group ownership of a file:
- `chgrp group file.txt`: Changes the group of `file.txt` to `group`.

#### `umask`

Sets default file permissions:
- `umask 022`: Ensures new files are created with `644` permissions.

#### `su`

Switches to another user:
- `su user`: Switches to the specified user.

#### `adduser`

Adds a new user:
- `adduser username`: Creates a new user account named `username`.

#### `deluser`

Deletes a user:
- `deluser username`: Removes the user `username`.

#### `who`

Displays information about logged-in users:
- `who`: Lists all users currently logged in.

#### `last`

Shows the login history of users:
- `last`: Displays a list of user logins.

### Processes Management

#### `htop`

An interactive process viewer, offering a detailed overview of system processes and the ability to manage them directly.

- `htop`: Opens a visual overview of system processes.

#### `kill`

Terminates processes by PID or name:
- `kill 533494`: Terminates the process with PID `533494`.
- `killall firefox`: Terminates all processes named `firefox`.

#### `ps aux`

Displays all running processes:
- `ps aux`: Lists all running processes.
- `ps aux | grep firefox`: Filters the list of processes for "firefox".

#### `nohup` COMMAND &

Executes a command that continues running in the background, immune to hangup signals.
- `nohup python script.py &`: Runs script.py in the background, ensuring it continues even if the terminal closes.

#### `Ctrl+Z`

Suspends a foreground process, moving it to the background in a stopped state. This allows the terminal to return to the prompt for other tasks while keeping the process's state intact.

#### `jobs`

Lists active or suspended jobs:
- `bg %1`: Resumes job 1 in the background.
- `fg %1`: Brings job 1 to the foreground.

### System Resource and Information

#### `du`

Displays disk usage for files and directories:
- `du -h /path/to/directory`: Displays human-readable sizes.
- `du -sh /path/to/directory`: Displays the size of the directory.
- `du -a -h /path/to/directory`: Displays sizes of all files and directories.
- `du -h --max-depth=1 /path/to/directory`: Limits depth to show directory sizes.

#### `df`

Displays available disk space:
- `df -h`: Shows disk space in a human-readable format.
- `df -m`: Shows disk space in mega bytes.

#### `free`

Shows memory usage:
- `free -h`: Displays memory usage in a human-readable format.

#### `top`

Displays real-time process activity.

#### `ncdu`

Interactive tool to analyze disk usage.

#### Cleanup files

- Clean up the APT cache:
  - List disk usage: `sudo du -sh /var/cache/apt`
  - Clean: `sudo apt clean`
- Remove packages you no longer need: `sudo apt autoremove`
- Cleanup journal logs:
  - List disk usage: `journalctl --disk-usage`
  - Remove logs from x-time: `sudo journalctl --vacuum-time=7d`
- Clear thumbnail cache:
  - List disk usage: `du -sh ~/.cache/thumbnails`
  - Clear: `rm -rf ~/.cache/thumbnails`
- Clear temporary files:
  - List disk usage: `sudo du -sh /tmp`
  - Clear: `sudo rm -rf /tmp/*`

### System Health and Uptime

#### `uptime`

Shows system uptime and load averages:
- `uptime`: Displays the current uptime.
- `uptime -s`: Shows the last boot time.

#### `uname`

Displays system information:
- `uname -a`: Displays all system information.

### Time and Date Management

#### `date`

Displays or sets the system date and time:
- `date`: Shows the current date and time.

#### `timedatectl`

Manages system time, date, and time zones:
- `timedatectl`: Displays current time settings.

### System Control

#### `shutdown`

Safely shuts down or reboots the system.
  - `shutdown now` (Shuts down the system immediately.)
  - `shutdown -r +10` (Reboots the system after a 10-minute delay.)

#### `reboot`

Reboots the system immediately.

#### `systemctl`

Controls the systemd system and service manager.
- `systemctl status`: Shows the status of all active systemd units.
- `systemctl status NetworkManager`: Shows the status of the `NetworkManager` service.
- `systemctl enable <service>`: Enables a service to start automatically at boot.
- `systemctl disable <service>`: Disables a service from starting at boot.
- `systemctl restart <service>`: Restarts a service.

#### `env`

Displays environment variables in the current shell session:
- `env`: Lists all environment variables.

#### `export`

Sets environment variables in the current shell session:
- `export VAR=value`: Sets the value of `VAR`.

#### `unset`

Unsets environment variables in the current shell session:
- `unset VAR`: Removes `VAR`.

#### `printenv`

Prints the value of a specific environment variable:
- `printenv PATH`: Displays the `PATH` variable.

## Networking Commands

> [!NOTE]
> - `ifconfig` is considered deprecated and replaced by the `ip` toolset.
> - `netstat` is considered deprecated and replaced by the `ss` toolset.

#### `ip addr`

List your IP addresses.

#### `ping`

Ping to a device.
  - `ping google.com` (Pings google.com.)

#### `ss -tulpn`

Displays detailed information about network connections, listening ports, and the programs (processes) using those ports.

#### `curl`

Transfers data using various protocols:
- `curl http://example.com`: Fetches the content of `example.com`.
- `curl -o output.html http://example.com`: Saves the content to `output.html`.
- `curl -O http://example.com/file.zip`: Saves the file with its original name (`file.zip`).
- `curl -T uploadfile.txt ftp://example.com/upload/`: Upload a file to a ftp server.
- `curl -u username:password https://example.com/api`: Upload using authenication.
- `curl -X GET https://api.example.com/resource`: Send a HTTP GET.
- `curl -X POST -d "key1=value1" https://api.example.com/resource`: Send a HTTP POST.

#### `wget`

Downloads files from the web:
- `wget http://example.com/file.zip`: Downloads `file.zip`.
- `wget -O my_files.zip http://example.com/file.zip `: Downloads `file.zip`.
- `-b`: Runs the download in the background.
- `--no-check-certificate`: Skips SSL certificate validation.
- `--spider`: Checks if a file or URL exists without downloading it.
- `--mirror`: Mirrors a website, creating a local copy.

#### `scp`

Copies files between systems:
- `scp file.txt user@remote:/path`: Copies `file.txt` to a remote system.
- `scp user@remote:/path/to/file.txt ./`: Copies `file.txt` from the remote system to the current directory.
- `scp -r localdir user@remote:/path/to/destination`: Recursively copies `localdir` to the remote system.

#### `ssh`

Connects to a remote system securely:
- `ssh user@hostname`: Logs into `hostname` as `user`.
- `ssh -A user@hostname`: Logs into `hostname` as `user` with ssh agent forwarding.
- `ssh user@hostname "ls -l /path/to/directory"`: Runs the command on the remote system and displays the output locally.
- `ssh-copy-id user@hostname`: Installs your public key on the remote system for passwordless login.
