# Add user to sudoers file

`sudo` is a command line programme that allows trusted users to execute commands as root user.

Running a `sudo` command will prompt for a password.  For local development in WSL, developers may prefer to add their WSL user to the `sudoers` file to avoid the need for a password.

## Update `sudoers` file

1. Open sudoers file with root permission to edit with `sudo nano /etc/sudoers` 
2. Append `username  ALL=(ALL) NOPASSWD:ALL` to bottom of file replacing `username` with your WSL username
3. Save and exit the file

