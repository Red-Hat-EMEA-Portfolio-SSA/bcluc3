# BCL Use Case 3

You need to login to quay.io to use our execution environment with an account that has access to it:

```
podman login quay.io
```

## For running the oneview playbook

Then you need to create a file with your usernames and passwords, something like this:

```
oneview_username: your_oneview_username
oneview_password: your_oneview_password
oneview_domain: your_oneview_domain
oneview_apiversion: your_oneview_api_version
```

In our case we've called that file secrets.yml, therefore our command to run the playbooks would be:

```
ansible-navigator run 0_check_firmware.yml -e @secrets.yml
```

## For running the esxi update and vmware preparation playbooks

Export the values variables for your vCenter:

```
export VMWARE_HOST=vcenter.fqdn.local
export VMWARE_USER=my-admin-user
export VMWARE_PASSWORD=my-admin-password
export VMWARE_VALIDATE_CERTS=true/false
```

Then create a file conaining the values for accessing the esxi hosts defined in the inventory:

```
ansible_ssh_user: "my-user"
ansible_ssh_pass: "my-password"
ansible_become_pass: "my-sudo-password"
```

In our case we've called that file secrets.yml, therefore our command to run the playbooks would be:

```
ansible-navigator run 2_update_esxi.yml -e @secrets.yml -e @vars.yml
```