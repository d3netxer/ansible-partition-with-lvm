# ansible-partition-with-lvm

Use Ansible to provision an Ubuntu Xenial (16.04) instance to partition an Ubuntu instance into multiple LVM partitions.

This assumes you launched an EC2 instance with an attached volume (tested on 10gb attached volume size).

## Steps

1. On your server clone this repo

2. Navigate to the script directory and run the ansible shell script ('source ansible.sh'). This will install ansible.

3. Navigate to the ansible directory and configure the pvs (physical volumes) variable in the main-step2.yml file.You can run the following command to find out the name of the attached volume: ```sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL```

4. Run the main-step1 playbook ('ansible-playbook main-step1.yml')



