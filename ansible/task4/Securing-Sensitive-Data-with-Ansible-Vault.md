
# Securing Sensitive Data with Ansible Vault

## Objectives

- Install and configure MySQL server using Ansible
- Create a database and user with privileges
- Secure sensitive data (DB password) using Ansible Vault
- Validate database setup

## Playbook: `playbook.yml`

```yaml
---
- hosts: all
  become: yes
  vars_files:
    - vault.yaml   # Contains db_user and db_password

  tasks:
    - name: Install MySQL server
      apt:
        name: mysql-server
        state: present
        update_cache: yes

    - name: Start and enable MySQL service
      service:
        name: mysql
        state: started
        enabled: yes

    - name: Create iVolve database
      community.mysql.mysql_db:
        name: iVolve
        state: present
        # Use Unix socket authentication for root
        login_unix_socket: /var/run/mysqld/mysqld.sock

    - name: Create MySQL user
      community.mysql.mysql_user:
        name: "{{ vault_db_user }}"
        password: "{{ vault_db_password }}"
        priv: "iVolve.*:ALL"
        state: present
        host: localhost
        # Use Unix socket instead of password for root login
        login_unix_socket: /var/run/mysqld/mysqld.sock
```

## Vault File: `vault.yaml`

```yml

vault_db_user: ivolve_user
vault_db_password: MyS3cur3P@ss

```

## Steps

1. **Encrypt Vault File** (if not already encrypted):

```bash
ansible-vault encrypt vault.yaml
```

2. **Run Playbook**:

```bash
ansible-playbook -i inventory playbook.yml --ask-vault-pass
```

3. **Verify Database**:

```bash
mysql -u ivolve_user -p -e "SHOW DATABASES;"
```

- ### Optional: To Edit the Vault Later
  
  ```bash
  ansible-vault edit vault.yml
  ```
  
  ## Notes

- Ensure **Python MySQL module (`python3-pymysql`)** is installed for Ansible modules to work.

- Use `become: yes` for tasks requiring root privileges.

- Vault ensures sensitive info like DB passwords are not exposed in playbooks.

- Use `ansible-vault view` to read encrypted files securely.

- Ensure MySQL service is running and accessible on the managed node.

- Use `login_unix_socket: /var/run/mysqld/mysqld.sock` to let Ansible connect as MySQL root via socket (no password needed)































