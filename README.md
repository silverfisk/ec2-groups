# ec2-groups

Ansible role that maintains AWS EC2 security groups according to ```ec2_groups``` variable.

Variables example
---------

```cat vars/myvars.yaml```

```yaml
# AWS Security Groups
ec2_groups:
  ssh_sg:
    rules:
      - proto: tcp
        from_port: 22
        to_port: 22
        cidr_ip:
          - 10.0.0.0/8
          - sg-12345678
    state: present            # absent removes the group, present creates
  myapp_sg:                   # We can loop over several items to create multiple SGs (ssh_sg & myapp_sg)
    rules:
      - proto: tcp
        from_port: 443
        to_port: 443
        cidr_ip:
          - 10.0.0.0/8
          - sg-123456
    state: present            # absent removes the group, present creates
```

Playbook example:
-----------------
```cat playbook.yaml```
```yaml
---

- name: Configure security groups
  hosts: localhost
  become: yes
  gather_facts: true
  pre_tasks:
    - name: "Include global application variables"
      include_vars: "vars/myvars.yaml"
      tags:
        - ec2_groups

  roles:
    - { role: ec2-groups, tags: ["ec2_groups"] }
```
