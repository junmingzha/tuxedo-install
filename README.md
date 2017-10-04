# tuxedo-install

# Steps

1. Build an EL7 host

```
[wmcdonal@management ~]$ ansible-playbook ansible/laptop-lab/plays/setup_vms.yml
```

2. Install pre-reqs

```
[root@server ~]# yum install unzip gcc java-1.8.0-openjdk.x86_64
```

3. Create a user

```
[root@server ~]# useradd tuxedo
[root@server ~]# passwd tuxedo
```

```
[wmcdonal@wmcdonal-x1 ~]$ ssh-copy-id tuxedo@server
```

3. Copy install media

```
[user@management ~]$ scp tuxedo122200_64_Linux_01_x86.zip tuxedo@server:
```

4. Extract install media

```
[tuxedo@server ~]$ unzip tuxedo122200_64_Linux_01_x86.zip 
```

5. Create Oracle inventory 

```
[root@server ~]# cat > /etc/oraInst.loc <<EOF
inventory_loc=/home/tuxedo/oraInventory
inst_group=
EOF

chown tuxedo:tuxedo /etc/oraInst.loc
chmod 664 /etc/oraInst.loc
```

6. Create the response file

```
[user@management]$ scp installer.properties tuxedo@server:
```


7. Set JAVA_HOME

```
[tuxedo@server ~]$ export JAVA_HOME=/usr/lib/jvm/jre
```

8. Execute the OUI

```
[tuxedo@server ~]$ ./Disk1/install/runInstaller.sh -responseFile ~/installer.properties -silent -waitforcompletion
```

**Note:** the following step does not appear to be necessary when running with the minimal response file. Some additional options in the installer.properties may require root.sh but this minimal install does not currently.

9. Run the root.sh to register to the host's  central OUI registry

