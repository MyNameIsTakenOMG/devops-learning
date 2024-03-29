# devops-learning
## Table of Contents
 - [DevOps Tools](#devops-tools)
 - [Linux Basics](#linux-basics)
 - [Networking Basics](#networking-basics)
 - [Applications Basics](#applications-basics)
 - [Git](#git)
 - [Jenkins](#jenkins)
 - [Docker](#docker)
 - [Kubernetes](#kubernetes)
 - [Microservices Architecture](#microserviecs-architecture)
 - [Ansible](#ansible)
 - [Terraform](#terraform)
 - [Python](#python)

## DevOps Tools
 - version control: git, github...
 - ci/cd pipeline: gitlab, jenkins...
 - container and container orchestration: docker, k8s...
 - infrastructure as code: terraform, cloudformation...
 - infrastructure configuration tool: ansible
 - monitoring and maintaining underlying infras: prometheus and grafana

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

## Applications Basics
 - overview: `compiled`: developed, compiled, executed, such as java, c, c++ ; `interpreted`: developed, executed, such as nodejs, python, ruby. for modern compilers, they first translate the source code into an intermediary `Byte Code`, then the interpreter uses this `Byte Code` to translate to `Machine Code`.
 - java
   - overview: JDK: develop(jdb: debug, javadoc: documentation), build(javac: compile, jar: archive code and libraries into a jar file), run(JRE: runtime env, java: loader to run the apps). **note:** before v9, jdk and jre are seperated
   - `export PATH=$PATH:/path/to/directory` at the end of the `.bashrc` file for the user, then `source ~/.bashrc`. for system-side setup, `sudo vi /etc/system`, add `PATH="$PATH:/path/to/directory"`, then `source /etc/environment` to apply the changes.
   - build & packaging: develop the code, then use `javac` to compile the code to an intermediary `Byte Code`, then we can run the `Byte Code` in anywhere that supports `JVM`. for packaging, use `jar cf <output filename> {...classes}`(a manifest file created at path: META-INF/MANIFEST.MF). then run `java -jar <jar file name>`. for document, `javadoc -d doc <source code>` **(when executing commands, be sure at the target path)** --> build tools to handle all these steps: Maven, Gradle, ANT(compile, package, document) 
 - nodejs - npm: `npm -v`, `npm search file`, `npm install file`, `package.json`(containing the metadata of the project), `node -e "console.log(module.paths)"`, `npm i file -g`. `built-in packages`: `/usr/lib/node_modules/npm/node_modules/`, `external packages`: `/usr/lib/node_modules`
 - python
   - overview: `python2`(2000-2010), `python3`(2008-present) (not backward compatibility guaranteed, careful when migration). `yum install python2`, `yum install python36`. `note`: `if __name__ == '__main__'`
   - pip: pip2, pip3 for different python versions. `pip -V` to find out the python version. `pip install <package>`. the package path: `usr/lib(lib64)/python2.7(python3.6)/<packages...>`. `pip show <package>`. to find out the packages, `python2 -c "import sys; print(sys.path)"`. requirements: `requirements.txt`, and then `pip install -r requirements.txt`(always specify the versions of packages). for upgrade or uninstall, `pip install <package> --upgrade`, `pip uninstall <package>`. other package managers: `easy_install`(no need for unpackaging), `wheels`(need for unpackaging)
## Git
 - overview: local & remote repos, init, `git config user.email sarah@example.com`, `git config user.name sarah`, `git log --name-only`,`git log --decorate`,`git log --graph --decorate`, `git log --one-line`, `git rebase -i`, `git cherry-pick`, `git reflog`, `git search/find: --before, --after, --grep, --author, -- <filename>, `, `git submodules`
 - branches
 - initialize remote repo 
## Jenkins
 - overview
 - prerequisites: CI/CD : `continuous delivery != continuous deployment`
 - installation
 - jenkins plugins and integrations
 - `a note for ssh:`
   - `ssh -i /home/mike/.ssh/jenkins_key -l mike -p 8022 jenkins-server help`
   - `-i` flag is used to point to mike's private SSH key. Remember, we have already added the public key in the Jenkins configuration.
   - `-l` is the login user which in our example is mike
   - `-p` is the port which we found out in the previous step to be 8022
 - systems administration: `backup`: `$JENKINS_HOME folder`(configuration files-->config.xml; jobs folder), `restore`, `monitor`, `scale`, `manage`
   - 3 ways to backup: `file system snapshot`, `plugins for backup`: (Thinbackup-->backup&restore), `write a shell script that backs up the jenkins instance`
 - pipelines:
   - `pipeline`, `agent any`(build agent), `stage(*){}`, `steps{*}`, `multi-stage pipeline`
   - `Note: Here localhost:8085 refers to our Jenkins Controller and the /prometheus/ metrics path is exposed by the prometheus-metrics plugin that we installed previously`
   - first time tried out `prometheus` and `grafana`
## Docker
 - overview
 - commands: `docker run <image>`, `docker ps`, `docker ps -a`, `docker stop`,`docker rm`,`docker images`,`docker rmi`,`docker pull`,(containers only live as long as the process inside is alive),`docker run <image> sleep 5`,`docker exec`,`docker run -d`, `docker attach <container_id>`
 - docker run
 - docker image
 - docker engine_storage and networking
 - registry
## Kubernetes
 - overview
 - setup
 - concepts and objects
 - yaml 
## Microservices Architecture
## Ansible
 - overview
 - concepts
 - conditions, loops & roles
## Terraform
 - overview
 - introduction of IaC
 - getting started
 - basics
 - terraform state
 - working with Terraform
## Python



