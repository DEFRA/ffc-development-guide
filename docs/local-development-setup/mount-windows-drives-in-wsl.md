# Mount Windows drives in WSL

WSL will automatically mount all Windows drives under the `/mnt/` directory followed by the drive letter.

For example, the `C:` drive will be accessible on `/mnt/c/`.

However, in order to allow WSL to change Windows file permissions, the drives need to be mounted with the `metadata` option.

## Setup

1 Create or edit WSL config file by entering the below terminal command.

```
sudo nano /etc/wsl.conf
```

2 Add the following content to the file, then save and exit.

```
[automount]
options = "metadata"
```

3 Reboot machine for changes to take effect.
