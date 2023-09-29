# RKE2 Ansible Role Runbook

## Edit the inventory file, add your VM IPs under the following groups:

```
[masters]
xx.xx.xx.xx               #IP_HERE

[agents]
xx.xx.xx.xx               #IP_HERE
```

## Tree structure from the role parent folder point of view:

### The inventory file should be located in the same directory of the role folder, check the tree below:

```
$ tree .

.
├── inventory
├── main.yaml
└── rke2
    ├── defaults
    │   └── main.yml
    ├── files
    │   └── cloud.conf
    ├── meta
    │   └── main.yml
    ├── README.md
    ├── tasks
    │   ├── cloud_tasks.yml
    │   ├── main.yml
    │   ├── rke2_agent.yml
    │   ├── rke2_common.yml
    │   ├── rke2_master.yml
    │   └── setup_tasks.yml
    ├── templates
    │   ├── agent_config.yaml.j2
    │   └── master_config.yaml.j2
    ├── tests
    │   ├── inventory
    │   └── test.yml
    └── vars
        └── main.yml

```

## You need to insert the private IP of the master node into a variable located in ./defaults/main.yml:

```
rke2_master_private_ip: xx.xx.xx.xx                       #MASTER_PRIVATE_IP_HERE
```

#### Default values file located here , see the tree below:

```
$ tree ./rke2/defaults/ 

./rke2/defaults/
└── main.yml
```

## Check the connectivity (SSH) to the hosts defined in the inventory using the ping/pong:

```
$ ansible all -i inventory -m ping
```

## The Role itself needs to be referenced in a playbook as below , this playbook needs to be in the same directory beside the inventory file and the Role folder itself:


```
- hosts: all
  become: true
  roles:
    - ./rke2
```

### Check the placing of the main.yml which comtains the playbook that will trigger the role execution:


```
$ tree . -L 1

.
├── inventory
├── main.yaml
└── rke2

```


## Execute the role using the following command:

```
$ ansible-playbook main.yaml -i inventory 
```