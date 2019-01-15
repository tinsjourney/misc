# rh_reposync

This script will locally synchronize Red Hat Repository using RHSM credentials.

Create a default config.env file using the below example, in the same directory as the script.

```
# Set the following if you're using a proxy to reach Red Hat Network
PROXY_HOST=""
PROXY_PORT=""
PROXY_USER=""
PROXY_PASS=""

# RHSM credentials
RHSM_USER="tinsjourney@gnali.org"
RHSM_PASS="mypass"
RHSM_POOL="0123456789ABCDEF"

# Repository List
REPO_LIST="rhel-7-server-rpms \
        rhel-7-server-supplementary-rpms \
        rhel-7-server-rhv-4.2-manager-rpms \
        rhel-7-server-rhv-4-manager-tools-rpms
        rhel-7-server-ansible-2-rpms \
        jb-eap-7-for-rhel-7-server-rpms \
        rhel-7-server-rhv-4-mgmt-agent-rpms"
```
* Set your PROXY environment if you use a proxy to reach Red Hat website.
* Set you RHSM credentials
* REPO_LIST should list all the repository, available in your pool, you want to locally sync

Then you just need to launch the script as root. 

```
# ./rh_reposync.sh
```

By default the script will only download newest packages only, and delete local packages no longer present in repository, under /var/www/html/YYYYMMDD

If you want to download ALL packages present in Red Hat repository use the variable NEWEST_ONLY=false

```
# NEWEST_ONLY=false ./rh_reposync.sh
# ls /var/www/html/20190107/
jb-eap-7-for-rhel-7-server-rpms     rhel-7-server-rhv-4-manager-tools-rpms  rhel-7-server-rpms
rhel-7-server-ansible-2-rpms        rhel-7-server-rhv-4-mgmt-agent-rpms     rhel-7-server-supplementary-rpms
rhel-7-server-rhv-4.2-manager-rpms
```

If you want to put your config file in a different path, you can call the script using the CONFIG variable with the full path.

```
# CONFIG=/media/encrypted_usbkey/config.env /root/rh_reposync.sh
```

:warning:  This script will subscribe your host to Red Hat, so make sure it's not already registered and you have enough subscription. At the end of the script, the host is removed from Red Hat system.
