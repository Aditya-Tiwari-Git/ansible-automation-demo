```markdown
# Ansible Ad-Hoc Commands

## Overview
Ansible ad-hoc commands are quick, one-time tasks that you can run without writing a playbook. They are useful for simple operations like testing connectivity, managing services, and running shell commands on remote hosts.

## Inventory File
Make sure your `inventory` file is correctly configured with the IP addresses of your managed servers. This file should be located at `../inventory/inventory`.

### Example 1: Test Connectivity
Use the following command to test if Ansible can reach the managed nodes. This command uses the `ping` module, which simply returns a "pong" response if the connection is successful.

```bash
ansible -i ../inventory/inventory all -m ping
```

### Example 2: Create a File on Managed Nodes
To create a file named `ansible_testing` in the `/tmp` directory on all managed nodes, use the following command. This uses the `shell` module to execute the `touch` command.

```bash
ansible -i ../inventory/inventory all -m shell -a "touch /tmp/ansible_testing"
```

### Example 3: Check NGINX Status
To check the status of the NGINX service on all managed nodes, use the `shell` module with the `systemctl` command:

```bash
ansible -i ../inventory/inventory all -m shell -a "systemctl status nginx"
```

### Example 4: Install a Package
To install the `git` package on all managed nodes, use the `apt` module. This ensures the package is present:

```bash
ansible -i ../inventory/inventory all -m apt -a "name=git state=present"
```

### Example 5: Restart NGINX Service
To restart the NGINX service on all managed nodes, use the `service` module:

```bash
ansible -i ../inventory/inventory all -m service -a "name=nginx state=restarted"
```

### Example 6: Reboot All Managed Nodes
To reboot all managed nodes, use the `reboot` module:

```bash
ansible -i ../inventory/inventory all -m reboot
```

## Notes
- Replace `../inventory/inventory` with the path to your inventory file if it’s located elsewhere.
- The `all` keyword targets all hosts listed in the inventory file. You can replace it with a specific group or hostname if needed.
- Always ensure you have the necessary permissions to execute these commands on the remote nodes.
```

### **Summary**
This file provides a handy reference for running various Ansible ad-hoc commands, enabling quick tasks without the need to write a full playbook.
