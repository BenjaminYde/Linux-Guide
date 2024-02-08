# The Terminal

The terminal in Linux is an interface in which you can type and execute text-based commands. It allows for efficient management of the operating system and software, providing a direct way to interact with the system's kernel and services. Unlike graphical user interfaces (GUIs), the terminal provides a lightweight, more controlled, and scriptable way of interacting with the computer.

## Accessing the Terminal

**Graphical Method**: On most Linux desktop environments, you can open the terminal from the applications menu. Look for "Terminal".

**Keyboard Shortcut**: Many distributions allow you to open a terminal window by pressing `Ctrl + Alt + T`.

## File Management Commands

### Basic Commands

- `pwd`: Prints the path of the working directory.
- `cd`: Changes the working directory, abbreviation for 'change directory'.
  - `cd` home: Go to home directory.
  - `cd ..`: Go 1 directory up.
  - `cd ../..`: Go 2 directories up.
  - `cd ../../etc`: Go 2 directories up and go to the specified folder.
  - `cd ~`: Go to your home directory.
- `mkdir`: Creates a folder inside the working directory.
  - `mkdir dir1`: Create folder in working directory.
  - `mkdir dir1 dir2 dir3`: Create multiple folders in working directory.
  - `mkdir ~/Desktop/dir1`: Create folder in specific directory.
  - `mkdir -p dir1/dir2/dir3`: Create parented folders in working directory.
- `ls`: Lists files in the working directory.
  - `ls -l -a` (list all files in rows with all info)
- `touch`: Creates a new empty file.
  - Example: `touch output.txt`
- `>`: Writes the output of a command to a file.
  - Example: `pwd > output.txt`
  - Writes the path of the working directory to the text file. (Overwrites the whole file).
- `>>:` Writes output of a command to the end of a file (append).
  - Example: `pwd > output.txt`

### Viewing and Manipulating File Content

- `cat`: Stands for Concatenate. This command allows us to create single or multiple files, view the content of a file, concatenate files, and redirect output in the terminal or files.
  - Example: `cat output.txt` (reads content of `output.txt` to the terminal).
- `mv`: Moves a file into a directory.
  - Example: `mv output.txt dir1` (moves `output.txt` to dir1 directory).
  - `mv dir1/*` . (move everything from dir1 to the working directory).
- `cp`: Copies a file.
  - Example: `cp output.txt output2.txt`
- `rm`: Removes a file.
  - ` rm output.txt` (removes `output.txt`)
  - ` rm -rf dir1` (removes `dir1` and everything in it recusively)
- `chmod`: Lets you change the mode of a file (permissions) quickly. It has a lot of options available with it.
- `sudo`: Stands for “superuser do,” letting you act as a superuser or root user while running a specific command. It's crucial for installing software or editing files outside the user's home directory.
  - `sudo apt install gimp`
  - `sudo cd /root`
- `file`: Determines the file type of a given file, using magic numbers.
  - Example: `file image.jpg` (Displays the file type.)
- `stat`: Displays detailed information about a file or file system.
  - Example: `stat filename.txt` (Shows file details including inode, size, and permissions.)
- `ln` (hard and symbolic links): Creates links between files.
  - Example (symbolic): `ln -s source.txt link.txt`
  - Example (hard): `ln source.txt link.txt`

### Viewing Large Files and Differences

- `less`: Better for viewing large files, displaying the content of a file one page at a time.
- `head`: Displays the first few lines of a file. By default, it shows the first ten lines.
- `tail`: Displays the last few lines of a file. By default, it shows the last ten lines.
- `diff`: Compares the contents of two files line by line, outputting lines that do not match.
  - Example: `diff file1.txt file2.txt` (Shows the differences line by line.)
- `sort`: 
  - Alphabetically: `sort file.txt`
  - Numerically: `sort -n file.txt`
  - Reverse order: `sort -r file.txt`
  - By column (second column as example): `sort -k 2 file.txt`
  - Human-readable sizes (e.g., 1K, 1M): `sort -h file.txt`


### Editing and Compression

- `nano`: Opens and edits the specified file. Nano is a powerful text editor in a terminal.
  - Example: `nano output.txt`
- `zip`: Compresses files and directories.
  - Example: `zip myzip.zip output1.txt output2.txt`

### Organizing and Finding Files

- `tree`: Displays files in a tree structure.
  - Example: `tree -L 1`
- `find`: Searches for files and directories based on criteria like name, modification date, size, and permissions. It's also capable of executing commands on the files found.
  - `find . -name "*.py"`     
  (Finds all .py files in the current directory and sub directories).
  - `find . -iname "*.py"`    
  (insensitive search).
  - `find /home/user/projects -type d -name build`    
  (Find all directories named build in the /home/user/projects directory).
  - `find /home/user -mtime -7`   
  (Find files modified in the last 7 days in the /home/user directory).
  - `find /var/log -size +100M`   
  (Find files larger than 100MB in the /var/log directory).
  - `find /home/user/images -name "*.jpg" -exec mv {} /home/user/backup \;`   
  (Find all .jpg files in the /home/user/images directory and move them to /home/user/backup).
  - `find /path/to/dir -name "*.tmp" -exec rm {} \;`  
    (Find all .tmp files and delete them)
- `grep`: Stands for “global regular expression print”, a powerful command used for searching text using patterns.
  - `apt list | grep firefox` (Show all packages, but filter on the name “firefox”).
  - `grep "word" filename.txt` (Search for a specific word in a file)
  - `grep -r "word" /path/to/directory` (Search recursively in all files in a directory)
  - `grep -A 3 "pattern" filename.txt` (Search for a pattern and display 3 lines after the match)
  - `grep -i "pattern" filename.txt` (Search for a pattern ignoring case)
  - `grep -c "pattern" filename.txt` (Count the number of lines that match a pattern)

## General Commands

- `whoami`: Outputs the username of the current user.
- `echo`: Displays a line of text/string that is passed as an argument.
  - Example: `echo "Hello World"` (Displays "Hello World" in the terminal.)
- `clear`: Clears the terminal screen, making it blank.
- `man`: Shows the manual page for commands, providing detailed information about command usage and options.
  - Example: `man mkdir` (Displays the manual page for the mkdir command.)
- `info`: Shows a more in-depth guide for commands than the man command.
  - Example: `info mkdir` (Displays a more in-depth guide for the mkdir command.)
- `history`: Displays a list of commands previously entered in the terminal session.
  - `history` (Shows full command history.)
  - `history | tail -n 20` (Displays the last 20 commands.)
  - `history -c` (Clears the entire command history.)

## System Management Commands

### Processes Management

- `htop`: An interactive process viewer, offering a detailed overview of system processes and the ability to manage them directly.
- `kill`: Terminates processes by their process ID (PID) or name.
  - Examples:
    - `kill 533494` (- )Terminates the process with PID 533494.)
    - `killall firefox` (Terminates all processes named firefox (Note: kill firefox is not a standard syntax; use killall for process names))
- `ps aux`: Displays all running processes.
  - Examples:
    - `ps aux` (Shows all running processes.)
    - `ps aux | grep firefox` (Shows all running processes, filtered to include only those containing "firefox".)

### System Resource and Information

- `du`: Displays the amount of disk space used by files and directories
  - `du -h /path/to/directory` (All directories human readable inside of directory)
  - `du -a -h /path/to/directory` (All files + directories human readable inside of directory)
  - `du -h --max-depth=1 /path/to/directory` (Short and human readable of directory with max depth to search)
- `df`: Displays disk space usage for all mounted filesystems.
  - Example: `df -h` (Displays in a human-readable format.)
`free`: Shows the amount of free and used memory in the system.
  - Example: `free -h` (Shows memory information in a human-readable format.)
- `top`: Displays an ongoing view of process activity in real time.

### System Health and Uptime

- `uptime`: Shows how long the system has been running along with load averages.
  - Examples:
    - `uptime` (Basic usage, shows current uptime.)
    - `uptime -s` (Shows the system's last boot date.)
- `uname`: Displays system information such as the kernel name, version, and more.
  - Example: `uname -a` (Shows all system information.)

### Time and Date Management

- `date`: Displays or sets the system date and time.
  - Example: `date` (Shows the current date and time.)
- `timedatectl`: Controls the system time and date on systemd systems, also allows for setting time zones and configuring NTP.
  - Example: `timedatectl` (Displays the current time settings.)

### System Control

- `shutdown`: Safely shuts down or reboots the system.
x    - `shutdown now` (Shuts down the system immediately.)
    - `shutdown -r +10` (Reboots the system after a 10-minute delay.)
- `reboot`: Reboots the system immediately.
- `systemctl`: Controls the systemd system and service manager.
  - Example: `systemctl status` (Shows the status of all active systemd units.)


## Networking Commands

todo