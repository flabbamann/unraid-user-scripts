# unraid-user-scripts
A collection of scripts to be used with unraid [User Scripts](https://forums.unraid.net/topic/48286-plugin-ca-user-scripts/) plugin

To use a script copy the directory to `config/plugins/user.scripts/scripts/` on flash drive or add a new script via Web GUI and paste the content of `script`. 

>[!WARNING]
>These scripts work for me and my use cases. Inspect the code and don't run any scripts on important data without testing!

## docker_prune_images
Removes all images not referenced by any container, to free up disk space (docker.img). In the docker tab of the WebGUI these images show up as "orphan images" if advanced view is enabled. For me this script works better than the `delete_dangling_images` script that comes with User Scripts plugin.

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
### Restore
To restore single files from a backup, you can mount your backup repo like this:
```bash
ln -s /usr/bin/fusermount3 /usr/bin/fusermount
mkdir /mnt/bkp
restic -r /path/to/your/repo -p /path/to/your/password.txt mount /mnt/bkp
```

## restic_forget_and_check
Drops old snapshots and prunes data from the repo that was only referenced by dropped snapshots.
After this a check is run to make sure there is nothing wrong with the repo.
Depending on your repo size and hardware both operations can take a long time. 
I run this on a monthly schedule.
*Make sure no backups are planned during the forget and check run.*

### unlock
In case an operation was interrupted your repository may be locked. To unlock run the following command. 
⚠️*Make sure you are not unnecessarily killing a running backup job!*
```bash
restic -r /path/to/your/repo -p /path/to/your/password.txt unlock
```