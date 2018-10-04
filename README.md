# ansible-partition-with-lvm

Use Ansible to provision an Ubuntu Xenial (16.04) instance to partition an Ubuntu instance into multiple LVM partitions.

This assumes you launched an EC2 instance with an attached volume (tested on 5gb attached volume size). Current setting (are configurable):

- var: 2 Gib
- tmp: .8 Gib
- var/tmp: .3 Gib
- var/log: .5 Gib
- var/log/audit: .3 Gib
- home: 1 Gib
total allocated: 4.9 Gib

## Steps

1. On your server clone this repo

2. Navigate to the script directory and run the ansible shell script ('source ansible.sh'). This will install ansible.

3. Navigate to the ansible directory and configure the pvs (physical volumes) variable in the main-step2.yml file.You can run the following command to find out the name of the attached volume: ```sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL```

### Extra tips for AWS:

With AWS instances, EBS volumes are exposed as NVMe block devices. The device names are /dev/nvme0n1, /dev/nvme1n1, and so on. You can run the following command to find out the name of the attached volumes: ```sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL```

you may get an output like this:
```
NAME        FSTYPE    SIZE MOUNTPOINT                 LABEL
loop0       squashfs 87.9M /snap/core/5328            
loop1       squashfs 12.7M /snap/amazon-ssm-agent/495 
nvme0n1                 5G                            
nvme1n1                10G                            
└─nvme1n1p1 ext4       10G /  
```

From this you can tell that nvme1n1 is the physical volume that is mounted to your root, and that nvme0n1 is the attached physical volume. The next step is to format your attached physical volume with a command like this: ```sudo mkfs -t ext4 /dev/nvme0n1```

Do not mount the volume. You can now proceed with the next step.

4. Run the main-step1 playbook ('ansible-playbook main-step1.yml')



