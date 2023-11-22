# Ansible Configuration for Web and Database Servers

This Ansible project automates the setup of Nginx web servers and PostgreSQL database servers.

## Project Structure

The project is organized as follows:

- `ansible.cfg`: Ansible configuration file.
- `inventory/`: Hosts inventory file.
- `roles/`: Custom roles for setting up Nginx and PostgreSQL.
- `secret.yml`: Encrypted secrets using Ansible Vault.
- `setup.yml`: Main playbook to run the tasks.

## Requirements

- Ansible 2.9 or later.
- Docker installed on the control machine and managed nodes.
- Access to Docker daemon on managed nodes.

## Setup

### Configure Docker Inventory 
   Ensure your Ansible inventory file reflects your Docker container setup. For instance:

```ini
[nginx]
nginx-server ansible_connection=docker ansible_user=root ansible_python_interpreter=/opt/venv/bin/python3
nginx-server2 ansible_connection=docker ansible_user=root ansible_python_interpreter=/opt/venv/bin/python3

[postgres]
postgres-server ansible_connection=docker ansible_user=root ansible_python_interpreter=/opt/venv/bin/python3

[docker:children]
nginx
postgres
```

## Manual Docker Container Setup

To manually create and start Docker containers, use the following commands:
```
bash
docker run --name nginx-server -p 80:80 -d nginx
docker run --name nginx-server2 -p 8080:80 -d nginx
docker run --name postgres-server -e POSTGRES_PASSWORD=example -p 5432:5432 -d postgres:12
```


## Network Considerations:

Make sure Ansible can communicate with Docker containers. If containers are in a specific Docker network, configure Ansible to use that network.

## Environment Variables and Volumes:

When using Ansible to manage Docker containers, specify any necessary environment variables and volumes in your Ansible playbooks.


1. **Clone the Repository:**

   bash
   git clone [your-repository-url]
   cd [repository-name]


## Setting Up Ansible Vault:

Before running the playbooks, you'll need to create or use an existing Ansible Vault password file for decrypting secret.yml.

echo [your-vault-password] > .vault_pass.txt


##  Running the Playbooks:

Run Ansible playbooks as you would normally. Ensure that you target the correct Docker containers in your inventory.

ansible-playbook setup.yml


## Role Descriptions

    Nginx: Installs and configures Nginx with a custom template.
    PostgreSQL: Installs PostgreSQL and configures databases and users.


## Security

This project uses Ansible Vault for securing sensitive data. Always encrypt your sensitive data before committing to the repository.