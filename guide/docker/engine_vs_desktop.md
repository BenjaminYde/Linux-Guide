# Engine vs Desktop

## Docker Engine

Docker Engine is the open-source application that builds and runs containers. Think of it as the engine of a car, it does all the heavy lifting but doesn't have a flashy UI or dashboard.

**Docker Engine is fundamentally a Linux technology**. It was built to work directly with features of the Linux kernel, such as namespaces (for isolation) and cgroups (for resource limiting). This is why it runs natively on any Linux distribution without needing extra layers of virtualization.

## Docker Desktop

**Docker Desktop is a complete application for Windows and macOS**. Its entire purpose is to make the Linux-native Docker Engine run seamlessly on operating systems that lack the required Linux kernel features.

Windows and macOS cannot run the Linux Docker Engine directly. Docker Desktop solves this by managing a lightweight Linux virtual machine (VM) in the background. It installs, configures, and runs the Docker Engine inside this managed VM for you, so you don't have to. It acts as the bridge between your host OS and the Docker Engine.

It bundles all the necessary components into one package:

- Docker Engine (the core technology)
- The Docker CLI
- A graphical user interface (GUI) for managing containers, images, and volumes.
- Integration with the host operating system's networking and filesystem.
- And more ...

## Can Docker Desktop only run Linux containers?

This is mostly true for developers using Docker Desktop on Windows or Mac. The primary purpose of this setup is to build and run **Linux containers**, as the vast majority of server applications are deployed on Linux.
However, Microsoft has its own container technology called **Windows Containers**. These run Windows applications and require a Windows kernel. You can switch Docker Desktop into "Windows Containers" mode, but this is a separate world from the Linux containers you use with WSL2 and is far less common for general development.

## Licensing: Free vs. Paid

This is a common point of confusion. The licensing model depends on the product and your use case.

- **Docker Engine**: Completely free and open-source. If you're running it on a Linux server, you will never have to pay for it.
- **Docker Desktop**: Free for certain uses. It is free for personal use, education, non-commercial open-source projects, and small businesses (fewer than 250 employees AND less than $10 million in annual revenue). It requires a paid subscription for professional use in larger companies.

For most individual developers or small teams, Docker Desktop remains free to use.

## For Windows Users: The Role of WSL2

### What is WSL2?

For years, developers faced a dilemma: use Windows for its excellent desktop experience, office software, and gaming, or use Linux for its powerful command-line tools, package managers, and server-side development environment. Before WSL, the solutions were clunky:

Before WSL, the solutions were clunky:

- **Dual-booting**: Required rebooting your entire machine to switch operating systems.
- **Heavy Virtual Machines (VMs)**: Used tools like VirtualBox or VMware, which were slow to start, consumed significant RAM and CPU, and had poor integration with the Windows host.
- **Compatibility Layers**: Tools like Cygwin tried to emulate Linux behavior on Windows but were often incomplete and slow.

Microsoft created the **Windows Subsystem for Linux (WSL)** to solve this problem directly. The goal was to make Windows a first-class development environment for any platform.

- **WSL1** was the first attempt. It worked by translating Linux system calls into Windows system calls on the fly. It was clever but had performance and compatibility limitations because not every Linux call has a direct Windows equivalent.

- **WSL2** is a complete paradigm shift. Instead of translating calls, it runs a full, real Linux kernel inside a lightweight, optimized virtual machine that integrates deeply with Windows. This approach provides near-native performance and almost 100% system call compatibility, meaning the vast majority of Linux applications and tools run without a hitch.

In short, WSL2 exists to give you the power and tooling of a Linux environment directly within Windows without needing to Dual-boot.

:::tip
Checkout the Docker Desktop WSL2 documentation [here](https://docs.docker.com/desktop/features/wsl/).
:::

### Pros and Cons Compared to Native Linux

While WSL2 is incredibly powerful, it's essential to understand its trade-offs compared to running native Linux.

#### Pros

- **Seamless Integration**: This is the killer feature. You can access your Windows files from Linux (at /mnt/c/) and your Linux files from Windows. You can run a command like code . in a Linux terminal to open the project in VS Code running on Windows.
- **Convenience**: You get the best of both worlds without rebooting. Use your favorite Windows applications like the Adobe suite or Microsoft Office right alongside a genuine Linux development environment.
- **Good Performance**: For tasks happening inside the Linux environment (like running scripts, building projects, or using databases), the performance is almost the same like native Linux but not 100% ofcourse.
- **Run Linux GUI Apps**: Windows Subsystem for Linux (WSL) now supports running Linux GUI applications (X11 and Wayland) on Windows in a fully integrated desktop experience. More info [here](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps).

#### Cons

- **Cross-OS File System Performance**: This is the most critical limitation. Accessing files on the Windows filesystem (`/mnt/c`) from within WSL is very slow. For any I/O-heavy task (like `git status`, `npm install`, or code compilation), you must store your project files inside the WSL2 filesystem (e.g., `~/git/my-project`).
- **Limited Hardware Access**: Direct access to certain hardware like GPUs (for tasks other than CUDA/DirectML), serial ports, or specific USB devices can be more complex than on a native Linux installation.
- **Networking**: WSL2 operates behind a virtual network switch. While this works seamlessly for 99% of use cases, it can sometimes complicate advanced networking setups involving VPNs or specific localhost behaviors.

## Key Takeaways

- Docker Engine is the core technology and Docker Desktop is the product that makes it easy to use on Windows/Mac.
- On Windows, Docker Desktop's WSL2 backend is the best choice for compatibility.
- For better performance, ALWAYS store your source code in the WSL2 Linux filesystem, not on your Windows C: drive.
- Docker Desktop is free for small businesses and personal use, but has a paid subscription for larger enterprises.

:::tip
Running Linux natively in a dual-boot configuration is preferable because it eliminates the performance overhead of a virtualization or abstraction layer. This direct hardware access ensures optimal speed and resource management compared to running it on top of Windows.
:::