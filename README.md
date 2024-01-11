# devops-learning
## Table of Contents
 - [Linux Basics](#linux-basics)
 - [Networking Basics](#networking-basics)


## Linux Basics
 - Linux CLI
   - basic commands: echo, ls, cd, pwd, mkdir, multiple commands with ';'
   - directories commands: mkdir **-p** as/asd/asdf/..., rm -r /as/dir, cp -r my-dir /asd/target_dir
   - files commands: touch , cat/ cat > , cp f1 f2, mv f1 f2, rm f1
   - more commands:
     - user accounts: whoami, id(more details about the user), su user2(switch user), ssh user@12.3.2.4, sudo + command(borrow privileges from root user)
     - download files: curl, wget (both add option '-O' to save the response to a file)
     - check os version: `ls /etc/*release*`, `cat /etc/*release*`
 - VI (Vim) Editor:
   - vi f_name;
   - command mode/ insert mode: esc / i
   - command mode:
     - move around: arrow keys or HJKL
     - delete: x -> char, dd -> line
     - cope&paste: yy -> copy line, p -> paste
     - scroll up/down: ctrl+u, ctrl+d
     - command: ':'
     - save:  ':w (optional: file_name)'
     - quit(discard): ':q'
     - save+quit: ':wq'
     - find: '/+(chars)';  'n' to move the next occurance 
 - Package Management
   - RPM (Red Hat Package Manager): install/ uninstall/ query : rpm -i/e/q
   - YUM: high-level package manager using rpm --> install packages as well as their dependencies by querying the packages (path: etc/yum.repos.d/)
     - install: yum install + package
     - yum repos: yum repolist
     - package: yum list + package / yum remove + package / yum --showduplicates list + package / yum install + package-{version number}
 - Service Management
   - start service:
     - service httpd start
     - systemctl start httpd
   - stop service:
     - systemctl stop httpd
   - check status:
     - systemctl status httpd
   - config service to start / not start at startup:
     - systemctl enable httpd / systemctl disable httpd
   - config custom services or apps for systemctl to manage
     - under the path: `/etc/systemd/system`, create a unit file of custom service or app, eg. my_app.service (note the extension: `.service`)
     - in the unit file, there are many sections:
       - `[service]` : `ExecStart=the command to run custom service or app`
       - then, run `systemctl daemon-reload` to refresh all services in the system.
       - then, run `systemctl start custom_service_name` to run the custom service.
       - then add a new section `[install]` with `WantedBy=multi-user.target` to make sure custom service start after `multi-user.target` starts during the system boots up.
       - then run `systemctl enable custom_service_name` to make sure custom service starts during the system boots up.
       - other sections, like `[Unit]` for metadata with `Description=`.
       - and for `[service]`, there are others declaratives, `[ExecStartPre]`, or `[ExecStartPost]`. and if the custom service is expected to restart when crashing, then add another declarative `Restart=always`

## Networking Basics
 - Switching: a device used to create a network in which all computers will be connected (can only work within a network)
   - `ip link`: used to check out the `interface` of the host(computer), eg. `eth0`
   - `ip addr add  192.168.1.11/24 dev eth0`: assign ip address to a host
 - Routing: a device used to establish a connection between networks. It gets assigned multiple ips based on how many networks it connects
   - `route`: checkout the routing table
   - `ip route add 192.186.2.0/24 via 192.168.1.1`
 - Default Gateway: the `destination` is `0.0.0.0` or `default`. note: if the `gateway` is `0.0.0.0`, that means it doesn't need a gateway to the outside, only routing within the network.
 - a linux host as a router: expect setting routing entries for both networks, still not working, because by default ip packet will not be forwarded from one interface to another for the sake of security. The setting path `/proc/sys/net/ipv4/ip_forward`: 0(deny) --> 1(allow). Note: this setting doesn't guarantee the persistence of the change across reboots, so must config `/etc/sysctl.conf` : net.ipv4.ip_forward = 1
 - DNS Configurations on Linux
   - `cat >> /etc/hosts`: telling the localhost that a new entry: `{ip address} {hostname}`
   - `hostname`: check the host name of the localhost
   - each host has a local host file for name resolution, but a DNS server will be more efficient, all other hosts only need to look up the DNS server
   - `cat /etc/resolv.conf`: `nameserver {nameserver_ip_address}`
   - the order of localhost file and DNS server: `cat /etc/nsswitch.conf`--> `hosts: files dns`
   - note: `cat >> /etc/resolv.conf`--> `nameserver 8.8.8.8` (a google nameserver that knows about all websites on the internet)
   - `cat >> /etc/resolv.conf` --> `search {domain_name: mycompany.com}`, then `ping web` --> `ping web.mycompany.com`
   - Records types: A / AAAA/ CNAME
   - nslookup: `nslookup exmaple.myweb.com`: doesn't look up the local host file
   - dig: compared to nslookup, dig will return more details about the domain









