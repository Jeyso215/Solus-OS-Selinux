# **Solus-SEC: GrapheneOS-like Security for Solus OS**

A community-driven project to enhance Solus OS with advanced security features inspired by GrapheneOS. This repository provides tools, scripts, and documentation to implement SELinux, seccomp, and Linux namespaces on Solus OS for improved system hardening and application isolation.

---

## **Features**

- **SELinux Integration**: Enable and configure SELinux on Solus OS for fine-grained Mandatory Access Control (MAC).
- **Seccomp Profiles**: Predefined seccomp profiles to restrict system calls for applications and services.
- **Linux Namespaces**: Utilize namespaces to isolate processes and restrict access to system resources.
- **Sandboxing Tools**: Integration with tools like Firejail for application sandboxing.
- **Security Policies**: Custom SELinux policies tailored for Solus OS.
- **Documentation**: Step-by-step guides for configuring and troubleshooting.

---

## **Installation**

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/solus-sec.git
   cd solus-sec
   ```

2. **Install Dependencies**:
   ```bash
   sudo eopkg install -y gcc make libseccomp-devel
   ```

3. **Run the Setup Script**:
   ```bash
   sudo ./setup.sh
   ```

4. **Reboot and Verify**:
   ```bash
   sudo reboot
   sestatus  # Should show SELinux in enforcing mode
   ```

---

## **Configuration**

- **SELinux Policies**: Modify policies in the ```policies/``` directory and load them using:
  ```bash
  sudo semodule -i your_policy.pp
  ```
- **Seccomp Profiles**: Apply custom seccomp filters using:
  ```bash
  sudo seccomp-tools apply your_filter.sc
  ```
- **Namespaces**: Use the provided scripts to isolate processes.

---

## **Usage**

- **Sandbox Applications**:
  ```bash
  firejail --seccomp=/path/to/profile.sc --netns your_application
  ```
- **Verify Security Status**:
  ```bash
  sudo ausearch -m avc  # Check SELinux denials
  ```

---

## **Known Issues**

- SELinux may cause instability with certain applications.
- Solus OS is not designed to use SELinux by default, so some features may require manual configuration.

---

## **Contributing**

Contributions are welcome! Please submit pull requests or issues to help improve this project. If you're new to contributing, check out the [CONTRIBUTING.md](CONTRIBUTING.md) file for guidelines.

---

## **Acknowledgments**

- Inspired by GrapheneOS's security model.
- Built on top of Solus OS's foundation.
- Special thanks to the Solus community for their support.

---

This repository aims to bring advanced security features to Solus OS, making it a more secure option for desktop users while maintaining its simplicity and usability.
