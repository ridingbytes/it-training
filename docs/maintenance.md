# SENAITE Maintenance

The following sections assume mostly that the VM was built by the
`senaite.ansible-playbook`( https://github.com/senaite/senaite.ansible-playbook)


## Starting/Stopping SENAITE

The `supervisorctl` command allows to start/stop the SENAITE instances as well
as the ZEO server. It must be executed as `root` or with the `sudo` command.

```
sudo supervisorctl start senaitelims_zeoclient1
sudo supervisorctl stop senaitelims_zeoclient1
sudo supervisorctl restart senaitelims_zeoclient1
```


## Add a ZOPE Super User via the terminal

The instance scripts support to add a super-user via the terminal:

```
root@senaite:~# /home/senaite/senaitelims/bin/client_reserved adduser <username> <password>
2018-10-16 12:47:31 WARNING plone.behavior Specifying 'for' in behavior 'Related items' if no 'factory' is given has no effect and is superfluous.
Created user: <username>
```

This user will be created in the `acl_users` folder in the Zope root, e.g. `http://localhost:8080/acl_users/manage`


## Connectint directly to the Database

Zope allows to connect directly to the database using an instance script:

```
root@senaite:~# /home/senaite/senaitelims/bin/client_reserved debug
Starting debugger (the name "app" is bound to the top-level Zope object)
2018-10-16 12:52:21 WARNING plone.behavior Specifying 'for' in behavior 'Related items' if no 'factory' is given has no effect and is superfluous.

>>> portal = app.senaite
>>> portal.Title()
'SENAITE'
>>> portal.setTitle("SENAITE LIMS")
>>> import transaction
>>> transaction.commit()
>>> portal.Title()
'SENAITE LIMS'
```


## Backup

The database is located in the `data` folder of the `senaite` user.
The main database is called `Data.fs` and is located in the `filestorage` folder.
The binary large objects `BLOBs` are located in the `blobstorage` folder.

```
root@senaite:~# tree -L 2 /home/senaite/data
/home/senaite/data
└── senaitelims
    ├── blobstorage
    ├── client1
    ├── client2
    ├── client_reserved
    ├── filestorage
    └── zeoserver
```

A backup can be triggered with the `backup` command as the `senaite_daemon` user.
The `backup` command is located in the `/home/senaite/senaitelims/bin`.

```
root@senaite:~# sudo -H -u senaite_daemon /home/senaite/senaitelims/bin/backup
INFO: Created /home/senaite/data/senaitelims/backups
INFO: Created /home/senaite/data/senaitelims/blobstoragebackups
INFO: Please wait while backing up database file: /home/senaite/data/senaitelims/filestorage/Data.fs to /home/senaite/data/senaitelims/backups
INFO: Please wait while backing up blobs from /home/senaite/data/senaitelims/blobstorage to /home/senaite/data/senaitelims/blobstoragebackups
INFO: rsync -a  /home/senaite/data/senaitelims/blobstorage /home/senaite/data/senaitelims/blobstoragebackups/blobstorage.0
```

A restore can be triggerd with the `restore` command as the `senaite_daemon` user.
The `restore` command is located in the `/home/senaite/senaitelims/bin`.

```
root@senaite:~# sudo -H -u senaite_daemon /home/senaite/senaitelims/bin/restore

This will replace the filestorage:
    /home/senaite/data/senaitelims/filestorage/Data.fs
This will replace the blobstorage:
    /home/senaite/data/senaitelims/blobstorage
Are you sure? (yes/No)? yes
INFO: Please wait while restoring database file: /home/senaite/data/senaitelims/backups to /home/senaite/data/senaitelims/filestorage/Data.fs
INFO: Restoring blobs from /home/senaite/data/senaitelims/blobstoragebackups to /home/senaite/data/senaitelims/blobstorage
INFO: rsync -a  --delete /home/senaite/data/senaitelims/blobstoragebackups/blobstorage.0/blobstorage /home/senaite/data/senaitelims
```


## Connecting to the VM with Port forwarding

Sometimes it is desireable to access an instance directly. However, since the
instances listen only on the local IP 127.0.0.1, this is not possible.

Here we can leverage the port forwarding capabilities of the `ssh` command.

```
ssh <SERVER IP> -L <local port>:<server listen addreess>:<server listen port>
ssh senaite@192.168.101.20 -L 8083:localhost:8083
```


## Checking for open ports

Use this command to check which ports are open from the outside of your server:

```
sudo nmap -Pn -sT -p T:1-65536 <SERVER IP>
```

Where:

    -Pn: Treat all hosts as online -- skip host discovery
    -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
    -p <port ranges>: Only scan specified ports
      Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9


Use this command to check which ports are open inside your server:

```
sudo netstat -tulpn
```

Where:

    -t: show TCP connections
    -u: show UDP connections
    -l, --listening
      Show only listening sockets.  (These are omitted by default.)
    -p, --program
      Show the PID and name of the program to which each socket belongs.
    -n, --numeric
      Show numerical addresses instead of trying to determine symbolic host, port or user names.
