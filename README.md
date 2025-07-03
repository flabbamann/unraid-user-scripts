# unraid-user-scripts
A collection of scripts to be used with unraid [User Scripts](https://forums.unraid.net/topic/48286-plugin-ca-user-scripts/) plugin

To use a script copy the directory to `config/plugins/user.scripts/scripts/` on flash drive or add a new script via Web GUI and paste the content of `script`. 

>[!WARNING]
>These scripts work for me and my use cases. Inspect the code and don't run any scripts on important data without testing!

## nextcloud_generate_previews
Runs a background job via occ command to pre-generate previews. Works with [linuxserver/nextcloud](https://github.com/linuxserver/docker-nextcloud), not tested with other container images.

## restic_backup
Creates backups using [restic](https://github.com/restic/restic).
If you don't already have restic available on your Unraid system:
- download the binary
- place it on your flash drive in the folder `custom/restic/`
- run the following commands (and add them to your `go` script, so restic survives a reboot):
```bash
# copy and update restic binary
cp /boot/custom/restic/restic /usr/local/bin/restic
chmod +x /usr/local/bin/restic
```
Then init a restic repo by running 
```bash
restic init --repo /path/to/your/repo
```
Save your password to a file and adjust the variables in the script to your needs.
I run this on a daily schedule.
