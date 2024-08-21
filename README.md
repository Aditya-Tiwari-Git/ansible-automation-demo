### Project Overview: Ansible Automation Demonstration

This project aims to provide a comprehensive demonstration of Ansible, an open-source automation tool, by showcasing its usage, benefits, and practical implementation. The project will cover Ansible's role in the industry, its comparison with other automation tools like Puppet and Chef, and a practical guide on using Ansible with a simple playbook.

#### Repository Structure
```
ansible-automation-demo/
│
├── README.md
├── inventory
│   ├── inventory
├── playbooks
│   ├── ansibleProject.yml
├── tasks
│   ├── adhoc-commands.md
```

---

### 1. What is Ansible?

**Ansible** is a powerful automation tool used to automate IT infrastructure tasks such as configuration management, application deployment, and orchestration. It is known for its simplicity, agentless architecture, and ease of use, which makes it a popular choice among DevOps engineers.

### 2. Why is Ansible Used in the Industry?

Ansible is widely used for several reasons:
- **Agentless Architecture**: Unlike Puppet or Chef, Ansible does not require any agents to be installed on the managed nodes. This reduces overhead and simplifies management.
- **Simple YAML Syntax**: Ansible playbooks are written in YAML, which is easy to read and understand, even for those new to automation.
- **Extensibility**: Ansible supports a wide range of modules that can be used to manage various types of systems, including Linux, Windows, network devices, and cloud environments.
- **Integration with CI/CD Pipelines**: Ansible can be easily integrated into existing CI/CD pipelines, making it a valuable tool for continuous deployment and configuration management.

### 3. Ansible vs. Puppet vs. Chef

- **Learning Curve**: Ansible is easier to learn due to its simple, declarative syntax in YAML, while Puppet and Chef use Ruby-based DSLs that are more complex.
- **Agentless**: Ansible's agentless architecture reduces the need to install additional software on the nodes, unlike Puppet and Chef, which require agents.
- **Community Support**: Ansible has a large and active community, which contributes to a wide range of modules, plugins, and integrations.

### 4. Project Environment Setup

To demonstrate Ansible, we will set up a simple environment consisting of:
- **Master VM**: This VM will have Ansible installed and configured to manage other nodes.
- **Tester VMs**: These VMs will be used to test the automation scripts. These could be Linux or Windows VMs.


## **Step-by-Step Guide: Ansible Automation Demonstration**

### **1. Setting Up the Virtual Machines (VMs)**

#### **1.1. Requirements**
- **Hardware/Software**: 
  - A system with virtualization support (e.g., VirtualBox, VMware, or a cloud provider like AWS/Google Cloud).
  - Internet connection to install necessary packages.
  - A Linux-based host OS is recommended for the Ansible control node.

#### **1.2. Create VMs**
1. **Master VM (Ansible Control Node):**
   - **OS**: Choose a Linux distribution (e.g., Ubuntu, CentOS).
   - **Specs**: 1 CPU, 1 GB RAM, 10 GB HDD.
   - **Network**: Ensure the VM can communicate with the tester VMs via SSH.

2. **Tester VMs (Managed Nodes):**
   - **OS**: Linux (Ubuntu, CentOS) or Windows.[Ubuntu is preferred]
   - **Specs**: 1 CPU, 1 GB RAM, 10 GB HDD.
   - **Network**: Ensure these VMs can be accessed by the Master VM via SSH (Linux) or WinRM (Windows).

#### **1.3. Network Configuration**
- **Private Network**: Configure the VMs to be on the same private network to allow communication between the control node and managed nodes.

#### **1.4. SSH Setup (For Linux VMs)**
Certainly! Below is the updated guide with the specific commands you requested for Step 2.

---

**Setting Up Passwordless SSH Authentication**
Passwordless SSH allows secure, automatic login between the Master VM and Tester VMs without needing to enter a password every time. Below is a step-by-step guide for setting up passwordless SSH authentication between your Master VM and Tester VMs.

#### **Step 1: Generate SSH Key Pair on the Master VM**

1. **Generate SSH Key Pair**:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   - **Location**: Press `Enter` to accept the default location (`/home/ubuntu/.ssh/id_rsa`).
   - **Passphrase**: Press `Enter` to skip setting a passphrase.

2. **List the Keys**:
   ```bash
   ls /home/ubuntu/.ssh/
   ```
   - **Expected Output**:
     ```
     authorized_keys  id_rsa  id_rsa.pub
     ```

3. **View and Copy the Public Key**:
   ```bash
   cat /home/ubuntu/.ssh/id_rsa.pub
   ```
   - **Copy the output** (starting with `ssh-rsa`) to your clipboard.

---

#### **Step 2: Set Up the Tester VM for Passwordless SSH**

1. **Login to the Tester VM**:
   - From the Master VM, SSH into the Tester VM:
     ```bash
     ssh ubuntu@<tester-vm-ip-address>
     ```
   - Enter the password when prompted.

2. **Navigate to the `.ssh` Directory**:
   ```bash
   cd /home/ubuntu/.ssh/
   ```
   - **Explanation**: This directory holds SSH configuration files, including the `authorized_keys` file where public keys are stored.

3. **Edit the `authorized_keys` File**:
   - Open the `authorized_keys` file with a text editor:
     ```bash
     vim authorized_keys
     ```
   - **Paste the Public Key**: 
     - Paste the public key that you copied from the Master VM.
   - **Save and Exit**: 
     - Press `Esc`, type `:wq`, and press `Enter` to save the file and exit the editor.

4. **Set Permissions (if needed)**:
   ```bash
   chmod 600 /home/ubuntu/.ssh/authorized_keys
   ```
   - **Explanation**: This ensures that the `authorized_keys` file is readable only by the owner for security purposes.

5. **Logout from the Tester VM**:
   ```bash
   logout
   ```

---

#### **Step 3: Test Passwordless SSH Login**

1. **Test SSH Login from the Master VM**:
   ```bash
   ssh ubuntu@<tester-vm-ip-address>
   ```
   - **Expected Result**: You should log in without being prompted for a password.

---

#### **Step 4: Repeat for Additional Tester VMs**

- Repeat **Step 2** and **Step 3** for each additional Tester VM you want to set up for passwordless SSH.

---

#### **1.5. Install Python on Managed Nodes**
- Ensure Python is installed on all Linux managed nodes:
  ```bash
  sudo apt-get update
  sudo apt-get install python3
  ```

### **2. Installing Ansible on the Master VM**

#### **2.1. Update System Packages**
```bash
sudo apt-get update
sudo apt-get upgrade
```

#### **2.2. Install Ansible**
```bash
sudo apt-get install ansible
```

#### **2.3. Verify Ansible Installation**
```bash
ansible --version
```
You should see the version information and a successful installation message.

### **3. Setting Up the Inventory File**

#### **3.1. Create an Inventory Directory**
```bash
mkdir /home/ubuntu/inventory
cd /home/ubuntu/inventory
```

#### **3.2. Create an Inventory File**
Create a file named `inventory`:
```ini
192.168.1.101
192.168.1.102
```
Replace the IP addresses with the actual IPs of your tester VMs.

### **4. Running Ansible Ad-Hoc Commands**

#### **4.1. Test Connectivity**
Test if Ansible can reach the managed nodes:
```bash
ansible -i inventory all -m ping
```
You should see a "pong" response if the connection is successful.

#### **4.2. Create a Directory named testing in Testing VM**
```bash
mkdir /home/ubuntu/testing
```

#### **4.3. Run an Ad-Hoc Command**
Create a test file on all managed nodes:
```bash
ansible -i inventory all -m shell -a "touch /home/ubuntu/testing/ansible-testing"
```
#### **4.4. Testing an Ad-Hoc Command**
Go to Testing VM and type the command:-
```bash
cd /home/ubuntu/testing
```
and list the files
```bash
ls
```
You will be able to see ansible-testing file over there.

### **5. Writing and Executing an Ansible Playbook**

#### **5.1. Create a Directory for Playbooks**
```bash
mkdir ~/ansible-automation-demo
mkdir ~/ansible-automation-demo/playbooks
```

#### **5.2. Create the Ansible Playbook**
```bash
cd ~/ansible-automation-demo/playbooks
vim ansibleProject.yml
```

**File:** `ansibleProject.yml`
```yaml
---
- name: Install and start NGINX on test servers
  hosts: all
  become: yes
  
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present

    - name: Start NGINX service
      service:
        name: nginx
        state: started
```

#### **5.3. Execute the Playbook**
Run the playbook to install and start NGINX:
```bash
ansible-playbook -i /home/ubuntu/inventory/ /home/ubuntu/ansible-automation-demo/playbooks/ansibleProject.yml
```

### **6. Verification and Validation**

#### **6.1. Verify NGINX Installation**
On each tester VM, check if NGINX is installed and running:
```bash
sudo systemctl status nginx
```

#### **6.2. Access NGINX**
If the tester VMs are running a web server, open a browser and enter the IP address of any of the tester VMs to verify that NGINX is serving content.

### **7. Cleaning Up**

#### **7.1. Stop and Remove VMs**
If you no longer need the VMs, power them off and remove them from your virtualization tool.

#### **7.2. Remove Temporary Files**
Remove any temporary files created during testing:
```bash
rm home/ubuntu/inventory/inventory
rm -r /home/ubuntu/ansible-automation-demo/playbooks
```

### **8. Conclusion**

In this project, you have set up a basic Ansible environment, learned how to run ad-hoc commands, and created a simple playbook to automate the installation and management of the NGINX web server. This is a foundational project that can be expanded by adding more complex playbooks and incorporating Ansible roles for better organization.

### **9. Optional Enhancements**

- **Add More Playbooks**: Create additional playbooks to manage other services like Apache, MySQL, or Docker.
- **Implement Roles**: Structure your playbooks using roles for reusability and better organization.
- **CI/CD Integration**: Explore integrating your Ansible playbooks with a CI/CD pipeline using Jenkins or GitHub Actions.
