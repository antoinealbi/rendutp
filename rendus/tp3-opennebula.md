
## Question 1 & 2 - A. Database


![DB](/png/db.png)


## Question 3 


##### Commande :

```

sudo tee /etc/yum.repos.d/opennebula.repo <<EOF
[opennebula]
name=OpenNebula Community Edition
baseurl=https://downloads.opennebula.io/repo/6.8/RedHat/\$releasever/\$basearch
enabled=1
gpgkey=https://downloads.opennebula.io/repo/repo2.key
gpgcheck=1
repo_gpgcheck=1
EOF

```

##### Verif :

```
[vagrant@frontend ~]$ sudo dnf repoquery --repoid=opennebula
opennebula                                                                                                                   10 kB/s | 3.0 kB     00:00
docker-machine-opennebula-0:6.0.0.3-1.ce.el8.x86_64
docker-machine-opennebula-debuginfo-0:6.0.0.3-1.ce.el8.x86_64
opennebula-0:6.0.0.3-1.ce.el8.src
opennebula-0:6.0.0.3-1.ce.el8.x86_64
opennebula-common-0:6.0.0.3-1.ce.el8.noarch
opennebula-common-onecfg-0:6.0.0.3-1.ce.el8.noarch
opennebula-debuginfo-0:6.0.0.3-1.ce.el8.x86_64
opennebula-debugsource-0:6.0.0.3-1.ce.el8.x86_64
opennebula-fireedge-0:6.0.0.3-1.ce.el8.x86_64
opennebula-fireedge-debuginfo-0:6.0.0.3-1.ce.el8.x86_64
opennebula-flow-0:6.0.0.3-1.ce.el8.noarch
opennebula-gate-0:6.0.0.3-1.ce.el8.noarch
opennebula-guacd-0:6.0.0.3-1.2.0+1.ce.el8.x86_64
opennebula-guacd-debuginfo-0:6.0.0.3-1.2.0+1.ce.el8.x86_64
opennebula-java-0:6.0.0.3-1.ce.el8.noarch
opennebula-libs-0:6.0.0.3-1.ce.el8.noarch
opennebula-migration-0:6.0.0.3-1.ce.el8.noarch
opennebula-node-firecracker-0:6.0.0.3-1.ce.el8.x86_64
opennebula-node-firecracker-debuginfo-0:6.0.0.3-1.ce.el8.x86_64
opennebula-node-kvm-0:6.0.0.3-1.ce.el8.noarch
opennebula-node-lxc-0:6.0.0.3-1.ce.el8.x86_64
opennebula-node-lxc-debuginfo-0:6.0.0.3-1.ce.el8.x86_64
opennebula-provision-0:6.0.0.3-1.ce.el8.noarch
opennebula-provision-data-0:6.0.0.3-1.ce.el8.noarch
opennebula-rubygems-0:6.0.0.3-1.ce.el8.x86_64
opennebula-rubygems-debuginfo-0:6.0.0.3-1.ce.el8.x86_64
opennebula-sunstone-0:6.0.0.3-1.ce.el8.noarch
opennebula-tools-0:6.0.0.3-1.ce.el8.noarch
python3-pyone-0:6.0.0.3-1.ce.el8.noarch

```

__________________________________________________________________________
## Question 4, 5, 6 - B. OpenNebula



### 4

```
-------------------------------------------------------------------------------
[vagrant@frontend ~]$ sudo grep 'DB =' /etc/one/oned.conf
DB = [ BACKEND = "mysql",
# DB = [ BACKEND = "mysql",
------------------------------------------------------------------------------
```

### 5

```
-------------------------------------------------------------------------------
[vagrant@frontend ~]$ sudo cat /var/lib/one/.one/one_auth
oneadmin:password
-------------------------------------------------------------------------------

```

###6

```

------------------------------------------------------------------------------

[vagrant@frontend ~]$ sudo systemctl status opennebula
● opennebula.service - OpenNebula Cloud Controller Daemon
     Loaded: loaded (/usr/lib/systemd/system/opennebula.service; enabled; preset: disabled)
     Active: active (running) since Sun 2024-06-23 17:28:05 UTC; 7h ago
    Process: 6908 ExecStartPre=/usr/sbin/logrotate -f /etc/logrotate.d/opennebula -s /var/lib/one/.logrotate.status (code=exited, status=0/SUCCESS)
    Process: 6909 ExecStartPre=sh gzip -9 /var/log/one/oned.log-* /var/log/one/monitor.log-* /var/log/one/vcenter_monitor.log-* & (code=exited, status=127)
    Process: 6910 ExecStartPre=/usr/share/one/pre_cleanup (code=exited, status=0/SUCCESS)
   Main PID: 6911 (oned)
      Tasks: 96 (limit: 11111)
     Memory: 639.5M
        CPU: 8min 34.681s
     CGroup: /system.slice/opennebula.service
             ├─6911 /usr/bin/oned -f
             ├─6929 /usr/bin/ruby /usr/lib/one/mads/one_hm.rb -p 2101 -l 2102 -b 127.0.0.1
             ├─6953 /usr/bin/ruby /usr/lib/one/mads/one_vmm_exec.rb -t 15 -r 0 firecracker
             ├─6970 /usr/bin/ruby /usr/lib/one/mads/one_vmm_exec.rb -t 15 -r 0 kvm -p
             ├─6987 /usr/bin/ruby /usr/lib/one/mads/one_vmm_exec.rb -t 15 -r 0 lxc
             ├─7004 /usr/bin/ruby /usr/lib/one/mads/one_vmm_exec.rb -t 15 -r 0 lxd
             ├─7021 /usr/bin/ruby /usr/lib/one/mads/one_vmm_exec.rb -t 15 -r 0 kvm
             ├─7038 /usr/bin/ruby /usr/lib/one/mads/one_vmm_exec.rb -l deploy,shutdown,reboot,cancel,save,restore,migrate,poll,pre,post,clean,update_sg,sna>
             ├─7057 /usr/bin/ruby /usr/lib/one/mads/one_tm.rb -t 15 -d dummy,lvm,shared,fs_lvm,fs_lvm_ssh,qcow2,ssh,ceph,dev,vcenter,iscsi_libvirt
             ├─7080 /usr/bin/ruby /usr/lib/one/mads/one_auth_mad.rb --authn ssh,x509,ldap,server_cipher,server_x509
             ├─7095 /usr/bin/ruby /usr/lib/one/mads/one_datastore.rb -t 15 -d dummy,fs,lvm,ceph,dev,iscsi_libvirt,vcenter,restic,rsync -s shared,ssh,ceph,f>
             ├─7112 /usr/bin/ruby /usr/lib/one/mads/one_market.rb -t 15 -m http,s3,one,linuxcontainers,turnkeylinux,dockerhub,docker_registry
             ├─7129 /usr/bin/ruby /usr/lib/one/mads/one_ipam.rb -t 1 -i dummy,aws,equinix,vultr
             ├─7143 /usr/lib/one/mads/onemonitord "-c monitord.conf"
             ├─7159 /usr/bin/ruby /usr/lib/one/mads/one_im_exec.rb -r 3 -t 15 -w 90 firecracker
             ├─7172 /usr/bin/ruby /usr/lib/one/mads/one_im_exec.rb -r 3 -t 15 -w 90 kvm
             ├─7185 /usr/bin/ruby /usr/lib/one/mads/one_im_exec.rb -r 3 -t 15 -w 90 lxc
             ├─7198 /usr/bin/ruby /usr/lib/one/mads/one_im_exec.rb -r 3 -t 15 -w 90 lxd
             ├─7211 /usr/bin/ruby /usr/lib/one/mads/one_im_exec.rb -r 3 -t 15 -w 90 qemu
             ├─7224 /usr/bin/ruby /usr/lib/one/mads/one_im_exec.rb -l -c -t 15 -r 0 vcenter
             └─7233 ruby /var/lib/one/remotes/im/lib/vcenter_monitor.rb

Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
Jun 23 17:27:54 frontend.one opennebula[6911]: WARNING: MYSQL_OPT_RECONNECT is deprecated and will be removed in a future version.
-------------------------------------------------------------------------------
sunstone:
-------------------------------------------------------------------------------

[vagrant@frontend ~]$ sudo systemctl status opennebula-sunstone
● opennebula-sunstone.service - OpenNebula Web UI Server
     Loaded: loaded (/usr/lib/systemd/system/opennebula-sunstone.service; enabled; preset: disabled)
     Active: active (running) since Sun 2024-06-23 17:28:06 UTC; 7h ago
    Process: 7276 ExecStartPre=/usr/sbin/logrotate -f /etc/logrotate.d/opennebula-sunstone -s /var/lib/one/.logrotate.status (code=exited, status=0/SUCCESS)
    Process: 7278 ExecStartPre=sh gzip -9 /var/log/one/sunstone.log-* & (code=exited, status=127)
   Main PID: 7279 (ruby)
      Tasks: 21 (limit: 11111)
     Memory: 132.5M
        CPU: 20.314s
     CGroup: /system.slice/opennebula-sunstone.service
             └─7279 /usr/bin/ruby /usr/lib/one/sunstone/sunstone-server.rb

Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :threshold_high=>66,
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :support_fs=>["ext4", "ext3", "ext2", "xfs"],
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :oneflow_server=>"http://localhost:2474/",
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :routes=>["oneflow", "vcenter", "support", "nsx"],
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :private_fireedge_endpoint=>"http://localhost:2616",
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :public_fireedge_endpoint=>"http://localhost:2616",
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :webauthn_avail=>true,
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]:  :session_expire_time=>3600}
Jun 23 17:28:08 frontend.one opennebula-sunstone[7279]: --------------------------------------
Jun 23 17:28:09 frontend.one opennebula-sunstone[7279]: == Sinatra (v3.1.0) has taken the stage on 9869 for production with backup from Thin

-----------------------------------------------------------------------------------

```

## Question 7, 8, 9

Conf système : 

```

[vagrant@frontend ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0 eth1
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 9869/tcp 22/tcp 2633/tcp 4124/tcp 4124/udp 29876/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
  
```

8 : 

```
[oneadmin@kvm1 vagrant]$ sudo dnf list installed | grep qemu-kvm
qemu-kvm.x86_64                               17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-audio-pa.x86_64                      17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-block-blkio.x86_64                   17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-block-rbd.x86_64                     17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-common.x86_64                        17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-core.x86_64                          17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-device-display-virtio-gpu.x86_64     17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-device-display-virtio-gpu-pci.x86_64 17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-device-display-virtio-vga.x86_64     17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-device-usb-host.x86_64               17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-device-usb-redirect.x86_64           17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-docs.x86_64                          17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-tools.x86_64                         17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-ui-egl-headless.x86_64               17:8.2.0-11.el9_4.3                 @appstream
qemu-kvm-ui-opengl.x86_64                     17:8.2.0-11.el9_4.3                 @appstream


```

9 :

```

[oneadmin@kvm1 vagrant]$ sudo systemctl status libvirtd
○ libvirtd.service - libvirt legacy monolithic daemon
     Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; preset: disabled)
     Active: inactive (dead)
TriggeredBy: ○ libvirtd-ro.socket
             ○ libvirtd-admin.socket
             ○ libvirtd.socket
       Docs: man:libvirtd(8)
             https://libvirt.org/


```


## 10

```

[oneadmin@kvm1 vagrant]$ sudo firewall-cmd --query-port=22/tcp
yes
[oneadmin@kvm1 vagrant]$ sudo firewall-cmd --query-port=8472/udp
yes

```

## 11

```

[oneadmin@frontend vagrant]$ ssh oneadmin@10.3.1.21
Last login: Sun Jun 23 17:55:21 2024 from 10.3.1.11
[oneadmin@kvm1 ~]$


```

img : 

![ssh](/png/ssh.png)

![alt text](/png/sshint.png)

## 12

```

[oneadmin@kvm1 vagrant]$ ip link show vxlan_bridge
4: vxlan_bridge: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/ether 2a:a9:98:8a:c1:b6 brd ff:ff:ff:ff:ff:ff
[oneadmin@kvm1 vagrant]$ ip addr show vxlan_bridge
4: vxlan_bridge: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether 2a:a9:98:8a:c1:b6 brd ff:ff:ff:ff:ff:ff
    inet 10.220.220.201/24 scope global vxlan_bridge
       valid_lft forever preferred_lft forever
[oneadmin@kvm1 vagrant]$ sudo firewall-cmd --zone=public --list-interfaces
vxlan_bridge eth1 eth0
[oneadmin@kvm1 vagrant]$ sudo firewall-cmd --zone=public --query-masquerade
yes

```


## Arret a cette erreur : 


![error1](/png/error1.png) 

![error2](/png/error2.png)


Logs : 


```

Mon Jun 24 01:10:05 2024 [Z0][VM][I]: New state is ACTIVE  
Mon Jun 24 01:10:05 2024 [Z0][VM][I]: New LCM state is PROLOG  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: Command execution failed (exit code: 255): /var/lib/one/remotes/tm/ssh/clone frontend.one:/var/lib/one//datastores/1/2acf04a75dae0ad2f661533951eaeda0 10.3.1.21:/var/lib/one//datastores/0/10/disk.0 10 1  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: clone: Cloning /var/lib/one/datastores/1/2acf04a75dae0ad2f661533951eaeda0 in /var/lib/one/datastores/0/10/disk.0  
Mon Jun 24 01:10:10 2024 [Z0][TrM][E]: clone: Command " set -e -o pipefail  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]:  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: if [ -d "/var/lib/one/datastores/1/2acf04a75dae0ad2f661533951eaeda0.snap" ]; then  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: SRC_SNAP="2acf04a75dae0ad2f661533951eaeda0.snap"  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: fi  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]:  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: tar -C /var/lib/one/datastores/1 --transform="flags=r;s|2acf04a75dae0ad2f661533951eaeda0|disk.0|" -cSf - 2acf04a75dae0ad2f661533951eaeda0 $SRC_SNAP | ssh 10.3.1.21 "tar -xSf - -C /var/lib/one/datastores/0/10"" failed: oneadmin@frontend.one: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).  
Mon Jun 24 01:10:10 2024 [Z0][TrM][I]: Error copying /var/lib/one/datastores/1/2acf04a75dae0ad2f661533951eaeda0 to 10.3.1.21:/var/lib/one//datastores/0/10/disk.0  
Mon Jun 24 01:10:10 2024 [Z0][TrM][E]: Error executing image transfer script: INFO: clone: Cloning /var/lib/one/datastores/1/2acf04a75dae0ad2f661533951eaeda0 in /var/lib/one/datastores/0/10/disk.0 ERROR: clone: Command " set -e -o pipefail if [ -d "/var/lib/one/datastores/1/2acf04a75dae0ad2f661533951eaeda0.snap" ]; then SRC_SNAP="2acf04a75dae0ad2f661533951eaeda0.snap" fi tar -C /var/lib/one/datastores/1 --transform="flags=r;s|2acf04a75dae0ad2f661533951eaeda0|disk.0|" -cSf - 2acf04a75dae0ad2f661533951eaeda0 $SRC_SNAP | ssh 10.3.1.21 "tar -xSf - -C /var/lib/one/datastores/0/10"" failed: oneadmin@frontend.one: Permission denied (publickey,gssapi-keyex,gssapi-with-mic). Error copying /var/lib/one/datastores/1/2acf04a75dae0ad2f661533951eaeda0 to 10.3.1.21:/var/lib/one//datastores/0/10/disk.0  
Mon Jun 24 01:10:10 2024 [Z0][VM][I]: New LCM state is PROLOG_FAILURE

```