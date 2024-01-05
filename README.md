# devops-learning
## Table of Contents
 - [Linux Basics](#linux-basics)


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
    









