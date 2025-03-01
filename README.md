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