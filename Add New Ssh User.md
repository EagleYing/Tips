# Add New Ssh User

### Add new general user 

You can add new general user by this command

`adduser username`

 You will see new folder with file `authorized_keys` in `/home/username/.ssh`

### Granted root permission

To avoid insufficient authority type

`user mod -aG sudo username`

### Create ssh key

These operates are on your client not your server, type

`ssh-keygen -t rsa`

to create two ssh files. For mac user you will see new file `id_rsa` and `id_rsa.pub` under `/Users/username/.ssh`

For windows user its always under `C:\Users\username\.ssh`

### Trensfor ssh file

create new folder `.ssh	` under `/home/username` 

`mkdir .ssh`

Transfer `id-rsa` to your server under `/home/username/.ssh` by `scp` (secure copy) . For example

`scp \xxx\xxx\id_rsa remote_username@remote_ip:remote_file_or_folder`

Then delete the original file `authorized_keys` by `rm authorized_keys` and rename your `id_rsa` file

`mv id_rsa authorized_keys`

### Modify permission

So far you will see this information use commond `ll`

`-rw------- 1 root root 416 Jan 14 11:41 authorized_keys`

It means the owner is root. You have to modify file's read and write permission

`chmod 700 .ssh`

`chmod 600 .ssh/authorized_keys`

`cd ..` under `/home` then

`chmod 770 username`

### Edit configuration file

Back to the client under `/Users/username/.ssh`, add new file

`sudo vim config`

Type the following

```c
Host server
    HostName 101.132.37.59
    Port 22
    User username
    LocalForward 30572 xxx.xxx.x.x:3372
    IdentityFile ~/.ssh/id_rsa
```

### Connect to server

`ssh username@remote_ip`

You will enter your username path

`sudo su`

Then enter the password, you will enter the root dictionary~



