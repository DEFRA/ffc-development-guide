# WSL configuration

When using WSL, repositories should be cloned to the Linux filesystem rather than the Windows filesystem.
This will result in better performance and compatibility with Linux tools.

Should you need to interact with the Windows filesystem from WSL, all Windows drives will automatically be mounted under the `/mnt/` directory.

For example, the `C:` drive will be accessible on `/mnt/c/`.

## Windows permissions

In order to allow WSL to change Windows file permissions, the drives need to be mounted with the `metadata` option.

1. Create or edit WSL config file by entering the below terminal command.

```bash
sudo nano /etc/wsl.conf
```

2. Add the following content to the file, then save and exit.

```
[automount]
options = "metadata"
```

3. Restart WSL for changes to take effect.

## Proxy

If using a device that has a proxy server, such as a Defra device, you will need to configure WSL to work around the proxy to avoid network conflicts.

1. Create or edit a `.wslconfig` in your Windows profile directory

2. Add the following content to the file, then save and exit.

```
[wsl2]
networkingMode=mirrored
autoProxy=false
```

3. Restart WSL for changes to take effect.
