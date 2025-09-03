# **Solus-OS-SELinux: GrapheneOS-like Security for Solus OS**

A community-driven project to enhance Solus OS with advanced security features inspired by GrapheneOS. This repository provides tools, scripts, and documentation to implement SELinux, seccomp, and Linux namespaces on Solus OS for improved system hardening and application isolation.

---

## **Features**

- **SELinux Integration**: Enable and configure SELinux on Solus OS for fine-grained Mandatory Access Control (MAC)
- **Seccomp Profiles**: Predefined seccomp profiles to restrict system calls for applications and services
- **Linux Namespaces**: Utilize namespaces to isolate processes and restrict access to system resources
- **Sandboxing Tools**: Integration with tools like Firejail for application sandboxing
- **Security Policies**: Custom SELinux policies tailored for Solus OS
- **Documentation**: Step-by-step guides for configuring and troubleshooting

---

## **Important Considerations**

Solus OS does not natively support SELinux; it uses AppArmor as its default Linux Security Module (LSM). Implementing SELinux requires using a community-driven project, as this is not officially supported by the Solus team. This process involves compiling components from source and may affect system stability. Proceed with caution and ensure you have backups.

---

## **Installation Steps**

### 1. Prepare Your System

First, install necessary development tools and dependencies:

```bash
sudo eopkg install -c system.devel git coreutils
```

### 2. Clone the Community Repository

The Jeyso215/Solus-OS-Selinux project provides the necessary tools for SELinux integration:

```bash
git clone https://github.com/Jeyso215/Solus-OS-Selinux.git
cd Solus-OS-Selinux
```

This community-driven project enhances Solus OS with security features including SELinux, seccomp, and Linux namespaces.

### 3. Build and Install SELinux Components

Based on the repository structure, you'll need to build the components:

```bash
# Install kernel headers (required for SELinux kernel modules)
sudo eopkg install linux-current-headers

# Build and install SELinux userland tools
# Check the project's README.md for the exact build script name and process
```

*Note: The exact build process may vary depending on the current state of the repository. Always check the project's README.md for the most up-to-date build instructions.*

### 4. Configure SELinux

After installation, configure SELinux to your requirements:

```bash
# Set SELinux to permissive mode initially
sudo setenforce 0

# Create necessary directories
sudo mkdir -p /etc/selinux

# Copy or compile policies (check if policies need compilation from .te to .pp format)
# If policies/ contains source files, you may need to compile them first
sudo cp -r policies/ /etc/selinux/
```

### 5. Update Boot Configuration

**For UEFI Systems (systemd-boot - most Solus installations):**
```bash
# Edit the boot entry
sudo nano /boot/loader/entries/com.solus-project.current.conf

# Add "security=selinux" to the options line
# Example: options root=PARTUUID=... rw quiet security=selinux
```

**For BIOS Systems (GRUB):**
```bash
# Edit your boot loader configuration
sudo nano /etc/default/grub

# Add "security=selinux" to GRUB_CMDLINE_LINUX
# Example: GRUB_CMDLINE_LINUX="security=selinux"

# Update GRUB
sudo update-grub
```

### 6. Reboot and Verify

```bash
sudo reboot
```

After rebooting, verify SELinux is active:

```bash
sestatus
```

---

## **Configuration**

- **SELinux Policies**: Modify policies in the ```policies/``` directory and load them using:
  ```bash
  sudo semodule -i your_policy.pp
  ```
- **Seccomp Profiles**: Install seccomp-tools first, then apply custom seccomp filters:
  ```bash
  # Install seccomp-tools if not provided by the project
  sudo eopkg install seccomp-tools  # if available, otherwise build from source
  sudo seccomp-tools apply your_filter.sc
  ```
- **Namespaces**: Use the provided scripts to isolate processes.

---

## **Usage**

- **Install Firejail** (if not provided by the project):
  ```bash
  sudo eopkg install firejail
  ```
- **Sandbox Applications**:
  ```bash
  firejail --seccomp=/path/to/profile.sc --netns your_application
  ```
- **Verify Security Status**:
  ```bash
  sudo ausearch -m avc  # Check SELinux denials
  ```

---

## **Post-Installation Notes**

- Start with SELinux in permissive mode (```setenforce 0```) to monitor potential issues without enforcement
- Gradually transition to enforcing mode as you resolve any denials
- Monitor logs with ```sudo ausearch -m avc -ts recent```
- The community project may require ongoing maintenance as Solus updates
- This implementation is not officially supported by Solus and may have compatibility issues with future updates

---

## **Known Issues**

- SELinux may cause instability with certain applications
- Solus OS is not designed to use SELinux by default, so some features may require manual configuration
- Boot configuration differs between UEFI (systemd-boot) and BIOS (GRUB) systems

---

## **Contributing**

Contributions are welcome! Please submit pull requests or issues to help improve this project. Check the project's repository for contribution guidelines.

---

## **Acknowledgments**

- Inspired by GrapheneOS's security model
- Built on top of Solus OS's foundation
- Special thanks to the Solus community for their support

---

This repository aims to bring advanced security features to Solus OS, making it a more secure option for desktop users while maintaining its simplicity and usability.
