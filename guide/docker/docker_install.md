# Docker Install

## Linux

Compared with Windows, it's very simple because Docker is made for Linux and not Windows. You can find the official docker engine installation steps [here](https://docs.docker.com/engine/install/ubuntu/).

## Windows (using WSL)

### Install WSL2

Windows Subsystem for Linux (WSL) 2 is a full Linux kernel built by Microsoft, which allows Linux distributions to run without managing virtual machines. With Docker Desktop running on WSL 2, users can leverage Linux workspaces and avoid maintaining both Linux and Windows build scripts. In addition, WSL 2 provides improvements to file system sharing and boot time.

> [!NOTE]  
> You do not need Hyper-V when using WSL2. WSL2 is a separate virtualization technology that does not rely on Hyper-V. In fact, WSL2 uses a lightweight utility VM that leverages the Windows Hypervisor Platform (WHPX), which is a different component from Hyper-V.
>
> When you're using Docker Desktop with WSL2, you can run Linux containers directly on top of WSL2 without the need for Hyper-V. This provides better performance and resource usage compared to using Docker with the Hyper-V backend.
> 
> However, note that if you want to use both WSL2 and Hyper-V on the same Windows system, you can do so without any issues, as they can coexist without any conflicts.

> [!CAUTION]
> File system performance is critical for development. In WSL, the performance can be significantly lower when working on the Windows file system (mounted under /mnt/c/ or similar). To improve performance, move your code project to the Linux file system, which is typically faster.
> 
> This means you need to setup a ssh key in the folder "**.ssh**" and create a folder "**git**" in the home directory. (`~/.ssh` and `~/git`)

##### Method 1: When No WSL is installed

1. **Open PowerShell** or Windows Command Prompt in administrator mode by right-clicking and selecting "**Run as administrator**"
2. Enter `wsl.exe --install`.<br/>This command will enable the features necessary to run WSL and install the Ubuntu distribution of Linux
3. Restart PC

##### Method 2: When WSL 1 is installed, Upgrade to WSL 2

Follow the instructions from [here](https://learn.microsoft.com/en-gb/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)

### WSL Default Ubuntu Distro vs Custom Distro

When you install Ubuntu on Windows Subsystem for Linux (WSL), you can either choose the default Ubuntu distribution or a specific Long Term Support (LTS) release.
As Default "Ubuntu" is installed, this is the latest LTS distro of linux.
When running the `wsl -l` command, no version is explicitly specified in the name of the distro.
It is recommended to install a custom distro from the MS Store, these distro's have the version in the name.

### Install Ubuntu 22.04 LTS

1. Go to the MS Store
2. Search for "Ubuntu"
3. Download Ubuntu 22.04 LTS and Open it
4. Wait for installation and enter your username & pass
5. Installation is complete now, verify in windows powershell (as admin) with the following command: `wsl -l -v`
6. Set the Ubuntu 22.04 LTS as default distro with the following command: `wsl --set-default-version <distro-name>`
7. Remove the other Ubuntu distro with the following command: `wsl --unregister <distro-name>`

### Install Docker Engine

#### Method 1: Install Docker Desktop In Windows

1. Install docker engine from [here](https://www.docker.com/products/docker-desktop/)
2. Launch Docker Desktop, this should start docker engine
3. Verify that docker engine is running with the following command in the terminal: `docker version`

> [!CAUTION]
> Docker Desktop terms, Commercial use of Docker Desktop in larger enterprises (more than 250 employees OR more than $10 million USD in annual revenue) requires a paid subscription.


#### Method 2: Install Docker Engine In WSL (= No Docker Desktop)

1. Connect to WSL distro (ubuntu), via app or vis vs code wsl connection (via command palette)
2. Install docker ce (comminity edition) for ubuntu from the instructions [here](https://docs.docker.com/engine/install/ubuntu/)
3. Add current user to docker group 
    ```bash
    sudo groupadd docker
    sudo usermod -aG docker $USER
    ```
4. Make sure docker engine starts on boot <br/>
   - The following commands will open your /etc/sudoers file, which controls how sudo command are executed. <br/>
    We’ll want to allow our user to start dockerd without being prompted for a password. <br/>
    Add the following line to the bottom:<br/>
    ```bash
    sudo visudo
    ```
   - Add the following line to the file, make sure to change `<your-user-name>` to your user name
    ```bash
    <your-user-name> ALL=(ALL) NOPASSWD: /usr/bin/dockerd
    ```
   - Now, we will update our shell profile file to automatically run the docker daemon if it’s not running yet. <br/>
    This can be done by adding the following lines to your `.bashrc` or `.zshrc` <br/>

    ```bash
    # Start Docker daemon automatically when logging in if not running.
    RUNNING=`ps aux | grep dockerd | grep -v grep`
    if [ -z "$RUNNING" ]; then
        sudo dockerd > /dev/null 2>&1 &
        disown
    fi
    ```
   - Start docker engine manually with the following command: `sudo dockerd` 
   - Verify docker engine running with the following command: `docker images`, this will list all installed images.
   When no are installed it will be empty. When something is not installed correctly, an error will be thrown.

### SSH Agent Auto Start

To ensure that the SSH agent is always available for managing your SSH keys, it is a good practice to auto-start the agent when you open a new terminal session. By adding the necessary commands to your `.bashrc` file, the SSH agent will be automatically started, making it more convenient for you to use SSH keys when connecting to remote servers.

The `.bashrc` file is a shell script that Bash runs whenever it is started interactively, which means that it is executed every time you open a new terminal window. By adding the SSH agent startup commands to this file, you ensure that the agent is consistently available across your terminal sessions.

1. Open the `.bashrc` or `.zshrc` file with the following command `nano ~/.bashrc` or  `nano ~/.zshrc`
2. Add the following to the end of your ~/.bashrc file:
    ```bash
    # Auto add the .ssh private keys to the ssh agent
    if [ -z "$SSH_AUTH_SOCK" ]; then
    eval $(ssh-agent -s >/dev/null 2>&1)
    for key in ~/.ssh/id_*; do
        if [[ $key != *".pub" ]]; then  # Avoid public keys
        ssh-add "$key" >/dev/null 2>&1
        fi
    done
    fi
    ```
3. Save the file and exit the text editor.

Now, the SSH agent will be auto-started in every new terminal session, making it easier for you to manage and use your SSH keys.