# Ansible Playbook for setup 5G core

### All-in-one pre-setup flow:

```bash
sudo apt update && sudo apt install ansible sshpass git nano openssh-server -y
git clone https://github.com/simpledeadly/ngk-5g
cd ngk-5g
nano hosts.ini
ansible all -i hosts.ini -m ping
```

### lfg!

```bash
ansible-playbook -i hosts.ini setup.yaml -k
```