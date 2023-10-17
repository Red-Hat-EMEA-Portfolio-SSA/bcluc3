# BCL Use Case 3

You need to login to quay.io to use our execution environment with an account that has access to it:

```
podman login quay.io
```

## secrets.yml preparation
You need to provide a file with your usernames and passwords, something like this:

```
# Oneview access
oneview_host: ip_or_dnsname_of_oneview
oneview_username: your_oneview_username
oneview_password: your_oneview_password
# if you don not know your domain try "local" 
oneview_domain: your_oneview_domain 
oneview_apiversion: your_oneview_api_version

# esxi host access:
ansible_ssh_user: your_esxi_user
ansible_ssh_pass: your_esxi_password
ansible_become_pass: your_sudo_password
```
You should call the file secrets.yml because this filename is already ignored by git.

## Some credentials are expected as environment variables
You need to create a second file with usernames and passwords which you source before running the playbooks.

```
# vCenter Credentials
export VMWARE_HOST=ip_or_dnsname_of_vcenter
export VMWARE_USER=your_vcenter_user
export VMWARE_PASSWORD=your_vcenter_password
export VMWARE_VALIDATE_CERTS=false

# Ansible Controller Credentials - not needed 
# export CONTROLLER_HOST=ip_or_dnsname_of_aapcontroller
# export CONTROLLER_USERNAME=your_aap_user
# export CONTROLLER_PASSWORD=your_aap_password
# export CONTROLLER_VERIFY_SSL=false
```
We called this file env.txt, which is also ignored by git. 

Source the file into your shell environment to provide the variables to ansible:
```
. ./env.txt
```

## Run the different plays:

### Firmware related 
Display detailed firmware versions of the targeted systems:
```
ansible-navigator run 1a_oneview_check_firmware.yml -e @secrets.yml
```

Apply existing Service Pack to your targeted systems:
```
ansible-navigator run 1b_oneview_apply_spp.yml -e @secrets.yml -e @vars.yml
```

Check firmware compliance - this play fails when update is needed
```
ansible-navigator run 1c_firmware_compliance.yml -e @secrets.yml
```

### VMware related 
Update ESXi host to a newer version
```
ansible-navigator run 2_update_esxi.yml -e @secrets.yml -e @vars.yml
```

### VMs related 
Update ESXi host to a newer version
```
ansible-navigator run 3_gues_security_updates.yml -e @secrets.yml -e @vars.yml
```
HINT: within AAP we split this into 2 job-templates to provide different machine credentials for windows and RH