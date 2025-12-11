# Ansible Automation Playbooks

Production-ready Ansible playbooks for infrastructure automation and configuration management.

## 📦 Available Playbooks

- **Server Provisioning**: Initial server setup and hardening
- **Application Deployment**: Deploy web applications with dependencies
- **Database Setup**: PostgreSQL, MongoDB, MySQL installation and configuration
- **Security Hardening**: Firewall rules, SSH hardening, fail2ban
- **Monitoring Setup**: Install and configure monitoring tools

## 🚀 Quick Start
```bash
# Install Ansible
pip install ansible

# Run a playbook
ansible-playbook -i inventory/production playbooks/webserver-setup.yml

# Check syntax
ansible-playbook --syntax-check playbooks/database.yml
```

## 💼 Use Cases

- Automated server provisioning
- Consistent environment configuration
- Disaster recovery automation
- Compliance and security automation

[View Full Repository](https://github.com/jibran123/ansible-playbooks)