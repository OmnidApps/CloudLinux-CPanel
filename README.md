# CloudLinux / cPanel Setup and Migration - GCP VM Creation

## Phase 1: Creating the GCP VM Instance

### 1. Log in to the GCP Console

* Log in to the Google Cloud Console (console.cloud.google.com).
* **Screenshot 1:** [GCP Welcome Page](./img/welcome.png) - GCP Console landing page after login.

### 2. Navigate to Compute Engine

* Navigate to "Compute Engine" > "VM instances."
* **Screenshot 2:** [GCE VM Instances Page](./img/vm-creation-01.png) - Compute Engine VM instances page.

### 3. Create a New VM Instance

* Click the "CREATE INSTANCE" button.
* **Screenshot 3:** [Machine Configuration](./img/vm-creation-02.png) - Initial "Create an instance" page.

### 4. Configure the VM Instance

* Configure the VM instance with the following settings:
    * Name: `cpanel-cloudlinux-server`
    * Region and Zone: (Your selected region and zone)
    * Machine configuration: (Your selected machine type)
    * Boot disk: [Boot Disk Configuration](./img/vm-creation-04.png) -  Enterprise Linux (Rocky Linux/AlmaLinux/etc.), Standard Persistent Disk, 50GB+
    * Firewall: [Firewall Configuration](./img/vm-creation-05.png) - Allow HTTP and HTTPS traffic.
* Click "CREATE."
* **Screenshot 4:** [VM List Page](./img/vm-list-post-creation.png) - VM instances page showing the newly created instance.

## Phase 2: Connecting to the VM and Preparing the System

### 1. Connect via SSH

* Connect to the VM instance via SSH from the GCP Console.
* **Screenshot 6:** [SSH Connection](./img/instance-ssh.png) - SSH connection to the VM instance.

### 2. Update the System

* [Instance SSH](./img/instance-setup-01.png) - Ensure properly connected to shell.
* Update the system packages and reboot:
    
    ```bash
    sudo dnf update -y
    sudo reboot
    ```
* Reconnect to the VM via SSH after the reboot.

### 3. Verify SELinux Enabled

* Verify SELinux status:
    ```bash
    sestatus
    ```
* **Screenshot 7:** [SELinux Status](./img/selinux-status.png) - Output of `sestatus` showing SELinux is enabled.

### 4. Set the Hostname

* Set the hostname:
    ```bash
    sudo hostnamectl set-hostname your.domain.com
    ```
    (Replace `your.domain.com` with your actual domain.)
* Edit `/etc/hosts`:
    ```bash
    sudo nano /etc/hosts
    ```
    * Add the line: `<your-server-internal-ip> your.domain.com your`
    (Replace `<your-server-internal-ip>` with your server's internal IP.)
    * Save and exit.
* **Screenshot 8:** [Hosts File](./img/hosts-file.png) - Contents of `/etc/hosts` file.