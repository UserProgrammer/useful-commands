### Commands
- [git](#git)
- [postgres](#postgres)
- [vim](#vim)

### Tasks
- [Safely Remove USB-Attached Storage Device](#safely-remove-usb-attached-storage-device)

<br />

## Commands
### git
Create a tag in the current commit:
```shell
git tag -a tag-name -m "Tag description"
```

Move existing tag to current commit (description will not be carried over; must be explicitly included in the command):
```shell
git tag -f tag-name -m "Tag description"
```

Delete a tag:
```shell
get tag -d tag-name
```

Show tag details:
```shell
git show tag-name
```


Push a tag to the remote repository (include `-f` option if the tag already exists):
```shell
git push origin tag-name
```

</br>

### postgres
List all databases:
```shell
\list
```
Use a database for all subsequent queries:
```shell
\c database-name
```
List all tables:
```shell
\dt
```

</br>

### vim
Find out which plugin was the last to assign a value to a setting

```vim
:verbose set setting-name?
```

</br>

## Tasks
### Safely Remove USB-Attached Storage Device

```bash
# 1. Identify the device
lsblk

# 2. Unmount the device
udisksctl unmount -b /dev/sda2

# 3. Power-off the device
udisksctl power-off -b /dev/sda2
```
