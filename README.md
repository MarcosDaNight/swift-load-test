# swift-load-test


---

# Installing DevStack with OpenStack and Swift on Ubuntu 22.04 (VM in Bridge Mode)

## Add Stack User (optional)
DevStack should be run as a non-root user with sudo enabled (standard logins to cloud images such as “ubuntu” or “cloud-user” are usually fine).

If you are not using a cloud image, you can create a separate stack user to run DevStack with

```bash
sudo useradd -s /bin/bash -d /opt/stack -m stack
```

Ensure home directory for the stack user has executable permission for all, as RHEL based distros create it with 700 and Ubuntu 21.04+ with 750 which can cause issues during deployment.

```bash
sudo chmod +x /opt/stack
```

Since this user will be making many changes to your system, it should have sudo privileges:

```bash
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo -u stack -i
```

## Accessing the VM via SSH
- Make sure you can access your Ubuntu VM via SSH.

## Installing DevStack
1. Update the system packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install Git (if not already installed):
   ```bash
   sudo apt install git -y
   ```
3. Clone the DevStack repository:
   ```bash
   git clone https://opendev.org/openstack/devstack
   cd devstack
   ```
4. Create a configuration file `local.conf` in the `devstack` folder:
   ```bash
   nano local.conf
   ```
5. Insert the following content (replace `<PASSWORD>` with your desired password):
   ```ini
   [[local|localrc]]
   ADMIN_PASSWORD=<PASSWORD>
   DATABASE_PASSWORD=$ADMIN_PASSWORD
   RABBIT_PASSWORD=$ADMIN_PASSWORD
   SERVICE_PASSWORD=$ADMIN_PASSWORD
   HOST_IP=
   ```
6. Save the file and close the editor.
7. Start the DevStack installation:
   ```bash
   ./stack.sh
   ```

   - This will install DevStack. The process may take a considerable amount of time, depending on your internet connection speed.

## Configuring Access from Other Machines
1. Open the `local.conf` file again:
   ```bash
   nano local.conf
   ```
2. Add the following line to manually configure `HOST_IP` (leave it blank to auto-detect):
   ```ini
   HOST_IP=
   ```
3. Save and close the file.

## Downloading DevStack
- Clone the DevStack repository and navigate to the directory:
   ```bash
   git clone https://opendev.org/openstack/devstack
   cd devstack
   ```
   - The `devstack` repo contains a script that installs OpenStack and templates for configuration files.

## Restarting DevStack
```bash
./unstack.sh
./stack.sh
```

## Accessing the OpenStack Dashboard
- After the installation is complete, you should be able to access the OpenStack dashboard from any machine on your network. Use the VM's IP address in your browser.

---

Here is the modified `local.conf` with `HOST_IP` left blank:

```ini
[[local|localrc]]
ADMIN_PASSWORD=<YOUR_ADMIN_PASSWORD>
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP=
```

Remember to replace `<YOUR_ADMIN_PASSWORD>` with the password you want to use for the administrator user in OpenStack.

---



