# Ansible Node.js Deployment Project

This is a simple Ansible project to deploy a Node.js application on an Azure VM. 
The playbook installs Node.js, npm, PM2, clones the Node.js app from GitHub, installs dependencies, and starts the app using PM2.

Project Structure:

ansible-node-project/
├── inventory.ini      # Hosts configuration
└── deploy-node.yml    # Main Ansible playbook

Architecture Diagram:

Local Machine (Ansible + SSH) ---> Azure VM (Ubuntu 22.04, Node.js App, PM2 Process)

Prerequisites:

- An Azure VM running Ubuntu 22.04 or similar
- SSH access to the VM
- Node.js application repository on GitHub
- Ansible installed on your local machine (WSL, Linux, or MacOS)

Inventory File (inventory.ini):

[node-servers]
nodevm ansible_host=<VM_PUBLIC_IP> ansible_user=azureuser ansible_ssh_private_key_file=~/.ssh/node-vm.pem

Replace <VM_PUBLIC_IP> with your VM's public IP and update the SSH key path if needed.

Playbook (deploy-node.yml):

The playbook does the following:
1. Updates apt cache
2. Installs required packages (git, curl)
3. Installs Node.js LTS and npm
4. Clones the Node.js app from GitHub
5. Installs app dependencies
6. Installs PM2 globally
7. Starts the app using PM2

How to Run:

# Test connectivity
ansible -i inventory.ini node-servers -m ping

# Deploy the Node.js app
ansible-playbook -i inventory.ini deploy-node.yml

Verify Deployment:

1. SSH into the VM:
ssh -i ~/.ssh/node-vm.pem azureuser@<VM_PUBLIC_IP>

2. Check PM2 processes:
pm2 list

3. Visit in browser (default port 3000):
http://<VM_PUBLIC_IP>:3000

You should see:
Hello from Node.js app deployed by Ansible!

Notes:

- Ensure the app listens on 0.0.0.0 so it can be accessed externally
- Open the appropriate port in Azure Networking (default 3000)
- PM2 keeps the app running even after reboot

License:

MIT License
