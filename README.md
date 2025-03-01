# CloudLinux / cPanel Setup and Migration - GCP VM Creation

## Phase 1: Creating the GCP VM Instance

### 1. Log in to the GCP Console

* Log in to the [Google Cloud Console](console.cloud.google.com).
* **Screenshot:** [GCP Welcome Page](./img/welcome.png) - GCP Console landing page after login.

### 2. Navigate to Compute Engine

* Navigate to "Compute Engine" > "VM instances."
* **Screenshot:** [GCE VM Instances Page](./img/vm-creation-01.png) - Compute Engine VM instances page.

### 3. Create a New VM Instance

* Click the "CREATE INSTANCE" button.
* **Screenshot:** [Machine Configuration](./img/vm-creation-02.png) - Initial "Create an instance" page.

### 4. Configure the VM Instance

* Configure the VM instance with the following settings:
    * Name: `cpanel-cloudlinux-server`
    * Region and Zone: (Your selected region and zone)
    * Machine configuration: (Your selected machine type)
    * Boot disk: [Boot Disk Configuration](./img/vm-creation-04.png) -  Enterprise Linux (Rocky Linux/AlmaLinux/etc.), Standard Persistent Disk, 50GB+
    * Firewall: [Firewall Configuration](./img/vm-creation-05.png) - Allow HTTP and HTTPS traffic.
* Click "CREATE."
* **Screenshot:** [VM List Page](./img/vm-list-post-creation.png) - VM instances page showing the newly created instance.

## Phase 2: Connecting to the VM and Preparing the System

### 1. Connect via SSH

* Connect to the VM instance via SSH from the GCP Console.
* **Screenshot:** [SSH Connection](./img/instance-ssh.png) - SSH connection to the VM instance.

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
* **Screenshot:** [SELinux Status](./img/selinux-status.png) - Output of `sestatus` showing SELinux is enabled.

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
    
* **Screenshot:** [Hosts File](./img/hosts-file.png) - Contents of `/etc/hosts` file.

## Phase 3: Installing CloudLinux and cPanel

### Prerequisites:

1.  **CloudLinux License Key:** Obtain a valid CloudLinux license key from CloudLinux.
2.  **cPanel License Key:** cPanel licenses are typically automatically applied during install, but ensure you have a valid cPanel license.
3.  **Prerequisite Packages:**
    * `wget`: For downloading the CloudLinux installation script.

### Installation Steps:

1.  **Install wget (if needed):**
    * Ensure `wget` is installed:
        ```bash
        sudo dnf install wget -y
        ```

2.  **Install CloudLinux:**
    * Download the CloudLinux installation script:
        ```bash
        wget https://repo.cloudlinux.com/cloudlinux/sources/cln/cldeploy
        ```
    
    * Make the script executable:
        ```bash
        chmod +x cldeploy
        ```
    
    * Add user to `wheel` group
    
        ```bash
        sudo usermod -aG wheel $(whoami)
        ```
    
    * Run the CloudLinux installation script using your license key:
    
        ```bash
        sudo ./cldeploy -k <your-license-key>
        ```
        (Replace `<your-license-key>` with your actual license key.)

    * **Screenshot:** [CL Installing](./img/cl-installing.png)
    
    * Reboot the server:
    
        ```bash
        sudo reboot
        ```
    
    * Reconnect to the VM via SSH.
    
    * **Screenshot:** [CL Deployed](./img/cl-deployed.png) Verify CloudLinux is installed:
    
        ```bash
        cat /etc/redhat-release
        ```
    
3.  **Install cPanel:**
    
    * Navigate to the `/home` directory:
        ```bash
        cd
        ```
    * Download the latest cPanel installation script:
        ```bash
        wget https://securedownloads.cpanel.net/latest -O cpanel-latest
        ```
    * Make the script executable:
        
        ```bash
        chmod +x cpanel-latest
        ```
    * Run the installation script:
        
        ```bash
        sudo sh cpanel-latest
        ```
    * **Screenshot:** [CPanel Start Install](./img/cpanel-start-install.png) - Initial cPanel installation output.
    * **Screenshot:** [Screenshot 11.png](Screenshot 11.png) - Completed cPanel installation output.
    
4.  **Access cPanel and WHM:**
    * Access WHM at `https://<your-server-external-ip>:2087` or `https://your.domain.com:2087`.
        * (Replace `<your-server-external-ip>` with your external IP.)
        * (Replace `your.domain.com` with your FQDN.)
    * Log in with the `root` username and password.
    * **Screenshot:** [Screenshot 12.png](Screenshot 12.png) - WHM login page.
    * **Screenshot:** [Screenshot 13.png](Screenshot 13.png) - WHM dashboard.
