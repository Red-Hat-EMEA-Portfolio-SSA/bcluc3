# BCL Use Case 3

You need to login to quay.io to use our execution environment with an account that has access to it:

```
podman login quay.io
```

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