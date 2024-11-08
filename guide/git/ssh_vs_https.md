# SSH vs HTTPS

## Introduction

SSH (Secure Shell) and HTTPS (Hyper Text Transfer Protocol Secure) are both secure methods for accessing and interacting with remote repositories. While HTTPS is often used for web-based services, SSH is commonly utilized for secure communication between different systems. When it comes to working with remote Git repositories, both options have their merits, but SSH comes with certain advantages that might make it preferable.

## SSH

- **Secure communication**: SSH uses public-key cryptography to encrypt data transmitted between your local machine and the GitHub server. This ensures the confidentiality and    integrity of your code and any sensitive information during the transfer. 
- **Authentication without a password**: With SSH, you can authenticate using your SSH key pair instead of your GitHub password. This provides an additional layer of security, as you don't need to share your password with every Git operation. Furthermore, if someone gains access to your password, they still cannot access your repositories without your private SSH key. 
- **Simplified access management**: When using SSH keys, you can add, revoke, or manage access to your repositories without changing passwords. This can be particularly useful if you work with multiple computers or collaborate with others on your projects. 
- **Ease of Use After Initial Setup**: Once you set up your SSH keys and add your public key to your GitHub account, you can perform Git operations (e.g., clone, push, pull) without entering your username and password each time. This can save time and provide a more seamless experience when working with your repositories. 
- **Tunneling and Port Forwarding**: SSH allows secure tunneling of other protocols, meaning you can securely access resources within a private network, which HTTPS does not offer out-of-the-box.
- **Secure File Transfers**: SSH is the basis for other secure protocols like SFTP and SCP, which are often used for secure file transfers. Thus, if you're already set up to use SSH, this opens the door for additional secure operations beyond Git commands.
- **Scripting and Automation**: SSH can be easily integrated into shell scripts, facilitating automated, secure interactions with remote repositories.

## HTTPS

HTTPS is essentially an extension of HTTP, secured using TLS (Transport Layer Security) or SSL (Secure Sockets Layer). When a client connects to an HTTPS server, a handshake process occurs:

- **Server Identification**: The server sends a digital certificate to the client.
- **Certificate Validation**: The client verifies the certificate against a list of trusted Certificate Authorities (CAs).
- **Key Exchange**: The client and server exchange public keys to establish a unique session key for encryption.
- **Encrypted Session**: All data transferred between the client and server is encrypted using the session key.

**In the Context of Git**

When you clone, fetch, or push to a remote Git repository over HTTPS, the same encryption handshake happens. Your Git client and the Git server validate each other and establish an encrypted connection. For every operation, you might need to supply a username and password, or use credential storage to automate this.

### Personal Access Tokens (PATs)

When you have two-factor authentication (2FA) enabled or when the service requires it, you often can't use your regular password for Git operations over HTTPS. This is where PATs come in. They are tokens that can replace your password for various operations.

**How PAT Works**

- **Generation**: You generate a PAT from the service you are using (e.g., GitHub, GitLab, Bitbucket).
- **Permissions**: During generation, you set permissions or "scopes" that define what the PAT can do (read, write, delete repositories, etc.).
- **Use**: Instead of your password, you use this token in HTTPS Git operations.
- **Expiration**: Tokens can be set to expire after a certain period.

**Why Use PATs**

- **Security**: Even if someone gains access to your PAT, they can only perform actions according to the permissions you've set.
- **Revocation**: If you feel a token is compromised, you can revoke it independently without affecting other tokens or your main account password.
- **Specificity**: You can generate different tokens for different purposes, each with its own set of permissions.

## SSH vs HTTPS with GCM (Git Credential Manager) 

When using SSH for authentication with Git and GitHub, you don't need Git Credential Manager (GCM). SSH and GCM serve different purposes in the authentication process: 

- **SSH Authentication**: SSH (Secure Shell) uses public-key cryptography for authentication. When you set up an SSH key pair (private and public keys), you add the public key to your GitHub account. This allows you to interact with your repositories securely without entering your username and password each time. SSH authentication provides a secure and convenient way to authenticate without the need for GCM.
- **HTTPS with Git Credential Manager (GCM)**: GCM is a tool that helps manage and cache your Git credentials when using HTTPS for authentication. GCM stores your credentials so that you don't have to re-enter your username and password each time you perform Git operations. GCM is typically used in conjunction with HTTPS authentication. Which can pose a security risk if an attacker gains access to the machine where the credentials are stored. With SSH, you don't need to cache your credentials 

**How to clone**

- Clone using ssh: `git clone git@github.com:BenjaminYde/Linux-Guide.git`
- Clone using https: `git clone https://github.com/BenjaminYde/Linux-Guide.git`

## Generate SSH Key 

### Github

To use SSH with GitHub, you'll need to generate an SSH key pair (private and public keys) and add the public key to your GitHub account.

- On windows open <u>powershell</u>, on linux open the <u>terminal</u>
- **Create the .ssh directory** using `mkdir .ssh`
- Go into the directory using `cd .ssh`
- **Generate the .ssh key** using `ssh-keygen -t ed25519 -C "your_name@gmail.com"`
  - This will generate a private and public key.
    - Private key cannot be shared, is without the .pub extention
    - Public key is to be shared with Github
  - When prompted for a key name use the following convention name `id_ed25519_github`
  - When prompted for a password, you can press enter or give it a password
- **When the ssh-agent is not yet running, start it**.
  - Check if running using: `ssh-add -l`
  - **On Windows**
    - If not runnig, open <u>powershell as admin</u> and execute the following commands:
    ```powershell
    Set-Service   ssh-agent -StartupType Automatic
    Start-Service ssh-agent
    ```
  - **On Linux**
    - If not runnig, execute the following command: `eval $(ssh-agent -s)`
- **Add the ssh key to the agent** using `ssh-add <path-to-your-private-key>`
- Confirm if the key is added using `ssh-add -l`
- **Add the contents of the public key to Github**. 
  - To print the contents use `cat <path-to-your-public-key>`
  - On Github, go to Settings >> SSH >> New SSH key
  - Give it a name for example `laptop_linux`
  - Paste the contents of the public key in the description and save
- **On Windows: set git `core.sshCommand` global property to OpenSSH**
  ```bash
  git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
  ```
- **Test the connection** to github with `ssh -T git@github.com`
  - This should output something like:  
  Hi BenjaminYde! You've successfully authenticated, but GitHub does not provide shell access.
- **Open the command line and do a git clone** of a repo using ssh to verify everything works!

## Git and SSH in Windows

When working with Git on Windows, the integration of SSH is a crucial component for secure communication with repositories. The SSH setup can vary significantly due to the presence of different SSH clients and agents. This guide aims to provide clarity on configuring Git to work seamlessly with SSH, ensuring a smooth workflow in your development environment.

### Different SSH Versions

1. **Different SSH Versions**: On Windows, there can be multiple SSH clients available. The most common ones are:
   - **OpenSSH**: This is the open-source version of SSH, widely used across various operating systems, including Unix, Linux, and macOS. In recent versions of Windows 10 and later, OpenSSH has been included as an optional feature.
   - **PuTTY/Plink**: Another popular SSH client on Windows, often used before OpenSSH was readily available on the platform.
2. **Git's Default SSH Client**: When you install Git for Windows, it typically defaults to using its own bundled SSH executable (which is usually a version of OpenSSH). However, if you have multiple SSH clients installed (like PuTTY, WinSCP, or even another version of OpenSSH), Git might not use the one you expect.
```bash
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```
3. **Windows SSH Agent vs. OpenSSH Agent**:
   - **Windows SSH Agent** is a service in Windows that manages your SSH keys. It is integrated with the system and works across the board with different applications that use SSH.
   - **OpenSSH Agent** is similar but is specifically part of the OpenSSH suite. It might be preferable when working in environments consistent with Unix-like systems.
  
### Why Setting `core.sshCommand` Matters

1. **Consistency Across Environments**: By explicitly setting the SSH command in Git's configuration, you ensure that Git always uses the specific SSH client you want, regardless of what other SSH clients are installed on the system. This is crucial for consistency, especially in a team
environment where different members might have different setups.
1. **Key Management Compatibility**: Different SSH clients and agents manage keys differently. For example, keys added to the Windows SSH Agent might not be recognized by OpenSSH, and vice versa. By specifying `C:/Windows/System32/OpenSSH/ssh.exe`, you're directing Git to use the OpenSSH client, which will then use keys managed by the OpenSSH Agent.
2. **Avoiding Path Conflicts**: Windows might have multiple `ssh.exe` executables in different locations. Setting the path in the Git configuration avoids conflicts and ensures that the correct SSH executable is used.

## SSH Config

The SSH config file allows you to create shortcuts for servers you frequently access, as well as specify the options you wish to use with each. This can greatly simplify your workflow and enhance security by allowing you to specify key files, user names, and other options directly in the file, avoiding the need to enter them every time you make a connection.

#### Location of the SSH Config File

For individual users, the SSH config file is located at `~/.ssh/config` on Unix-like systems and Windows.

#### Creating and Editing the SSH Config File

If the file doesn't already exist, you can create it using a text editor. Make sure to set the correct permissions (read/write for the user, and not accessible by others) with `chmod 600 ~/.ssh/config` on Unix-like systems.

### Example Configuration

```
Host my_cool_server
    HostName 192.168.0.12
    User myusername
    IdentityFile ~/.ssh/id_ed25519
    ForwardAgent yes
```

- **Host**: A nickname for this host configuration. This is what you use in the ssh command (e.g., ssh myserver).
- **HostName**: The actual hostname or IP address of the server.
- **User**: The username to log in as on the remote server.
- **IdentityFile**: The path to the private key used for authentication with this server.
- **ForwardAgent**: This forwards your host ssh agent to the target device. This contains your current added ssh private keys. (handy to git clone on target)

Now you can connect to the server using the following command:

```bash
ssh my_cool_server
```

instead of:

```bash
ssh -A myusername@192.168.0.12
```

### Avoid Retyping Passwords

To avoid having to enter a password every time you connect to a target machine via SSH, you can add your public SSH key to the authorized_keys file of the target machine. This allows for passwordless authentication, assuming SSH key authentication is enabled on the target system.

```bash
ssh-copy-id -i ~/.ssh/id_ed25519_work.pub user@targetmachine
```

### Forward Environment Variables

In this tutorial, we'll show how to forward Git configuration environment variables when connecting to a remote machine via SSH. 
This is useful if your Git configuration is not set up on the remote device, but you still want to commit without copying your `.gitconfig` file, which could cause other users to commit under your name.
Instead of setting up Git configuration globally on the remote machine, we'll forward specific Git environment variables using SSH.

#### Step 1: Set Up Environment Variables Locally

First, make sure to add your Git-related environment variables to your `.bashrc` or `.zshrc` on your local machine. 
This ensures that these variables are set each time you start a new shell session.

```bash
# git
export GIT_AUTHOR_NAME="Benjamin Yde"
export GIT_AUTHOR_EMAIL="benjamin.yde@gmail.com"
export GIT_COMMITTER_NAME=$GIT_AUTHOR_NAME
export GIT_COMMITTER_EMAIL=$GIT_AUTHOR_EMAIL
```

#### Step 2: Configure SSH to Forward Environment Variables

Next, update your SSH configuration on the host (local) machine to forward these environment variables when you connect to the remote server.

```bash
# open the ssh config of your user
nano ~/.ssh/config

# add a new configuration block for the remote server, including the SendEnv directive to forward the environment variables:
Host my_cool_server
    HostName 192.168.0.12
    User myusername
    IdentityFile ~/.ssh/id_ed25519
    ForwardAgent yes
    SendEnv GIT_AUTHOR_NAME GIT_AUTHOR_EMAIL GIT_COMMITTER_NAME GIT_COMMITTER_EMAIL

# connect to the device using ssh
ssh my_cool_server
```

#### Step 3: Allow Environment Variables on the Remote Machine

```bash
# open the ssh server config file
sudo nano /etc/ssh/sshd_config

# append your environment variables
AcceptEnv GIT_AUTHOR_NAME GIT_AUTHOR_EMAIL GIT_COMMITTER_NAME GIT_COMMITTER_EMAIL
```

Finally, restart the SSH service on the remote machine to apply the changes:

```bash
sudo systemctl restart ssh
```

After completing these steps, your Git environment variables will be forwarded when you connect to the remote machine via SSH, allowing you to commit with your local configuration.

## Important notes

> [!CAUTION]
> - **Never Share or Store SSH Keys on Other Devices**: The private key is the cryptographic equivalent of a password. Storing it on other devices, especially those you do not fully control, exposes you to unnecessary risks. If you need to use SSH from different devices, each device should have its own key pair.
> 
> - **Use SSH Agent Forwarding**: If you need to SSH from one machine (A) to another (B), do not copy your private key to machine B.  
    Instead, use SSH Agent Forwarding to securely use the key from machine A on machine B without exposing it.
    To do this, use the `-A` flag when SSHing to the intermediate machine:
    ```bash
    ssh -A user@Machine_B_IP
    ```

## Handy Commands

**Q: How can I start using SSH in a repository where I'm currently using HTTPS?**

You'll need to update the origin remote in Git to change over from an HTTPS to SSH URL. Once you have the SSH clone URL, run the following command:

```bash
git remote set-url origin <SSH URL to your repository>
```