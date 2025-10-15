# Automated Web Server Configuration Using Ansible Playbooks

## Objectives

- Automate web server installation and configuration using Ansible
- Install and enable **Nginx** on a managed node
- Deploy a custom `index.html` page
- Verify that the web server is running correctly
```yaml
---
- name: Install and configure Nginx webserver
  hosts: server
  become: yes

  tasks:
    - name: install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes
        
    - name: Ensure Nginx service is started and enabled
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Copy custom index.html 
      copy:
        src: index.html
        dest: /var/www/html/index.html

    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```


## Steps

1. **Run Playbook**:

```bash
ansible-playbook -i inventory.ini playbook1.yml --ask-become-pass
```

- Use `--ask-become-pass` to enter sudo password.
2. **Verify Configuration**:

```bash
curl http://managed_node_ip
```

## Notes

- Ensure the managed node has **internet access** for package installation.

- Use `become: true` for tasks requiring root privileges.

- Firewall should allow **HTTP (port 80)**.

- Check that no other web server is running to avoid conflicts.

- ### Additional Notes:
   
- Instead of writing the HTML separated in file and pass it in the playbook using `copy`  module , you can but all in the playbook instead of that . 


                   
