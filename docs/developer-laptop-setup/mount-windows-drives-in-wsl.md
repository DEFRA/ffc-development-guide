# Overview
In order to access the Windows file system within WSL, each drive needs to be mounted.  By default WSL will bind these mounts to the path **/mnt/c/** where **c** is the C drive.

The problem with this is some service such as Docker Compose volumes need the binding to be **/c/**.  This can be configured in the WSL config file.

# Configuration
1. Create or edit WSL config file by entering the below terminal command.
`sudo nano /etc/wsl.conf`

1. Add the following content to the file, then save and exit.
    ```
    [automount]
    root = /
    ```
1. Reboot machine for changes to take effect.
