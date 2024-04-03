
# Ansible

Ansible is an open source configuration management and orchestration utility. It can automate and standardize 
the configuration of remote hosts and virtual machines. Its orchestration functionality allows Ansible to
coordinate the launch and graceful shutdown of multitiered applications. Because of this, Ansible can perform
rolling updates of multiple systems in a way that results in zero downtime.
Instead of writing custom, individualized scripts, system administrators create high-level plays in Ansible. A
play performs a series of tasks on the host, or group of hosts, specified in the play. A file that contains one or
more plays is called a playbook.
Ansible's architecture is agentless. Work is pushed to remote hosts when Ansible executes. Modules are the
programs that perform the actual work of the tasks of a play. Ansible is immediately useful because it comes
with hundreds of core modules that perform useful system administrative work.. More information on the Ansible [website](https://ansible.com/).

## Design Principles
## There are many things that Ansible can not do:
* Ansible can not audit changes made locally by other users on a system. For example, who made a change to a file?
 ## The following list provides some other examples of items that Ansible can not perform.
* Ansible can add packages to an installation, but it does not perform the initial minimal installation of the  system. Every system can start with a minimal installation, either via Kickstart or a base cloud starter image, then use Ansible for further configuration.
* Although Ansible can remediate configuration drift, it does not monitor for it. *
*  Ansible does not track what changes are made to files on the system, nor does it track what user or process made those changes. These types of changes are best tracked with a version control system or the Linux Auditing System.

## Ansible concepts and architecture
There are two types of machines in the Ansible architecture: the control node and managed hosts.
Ansible software is installed on the control node and all of its components are maintained on it. 
The managed hosts are listed in a host inventory, a text file on the control node that includes a list of managed host names or IP addresses.

## Ansible control node components
## Component Description
-------------------------------------

| Ansible configuration | Ansible has configuration settings that defines how it behaves. These settings include such things as the remote user to execute commands, and passwords to provide when executing remote commands with sudo. Default configuration values can be overridden by environment variables or values defined in configuration files.|
| ----------------------| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Host inventory        | The Ansible host inventory defines which configuration groups hosts belong to. The inventory can define how Ansible communicates with a managed host and it also defines host and group variable values. |
| Core modules | Core modules are the modules that come bundled with Ansible, There are over 400 core modules.            |
| Custom modules | Users can extend Ansible's functionality by writing their own modules and adding them to the Ansible library. Modules are typically written in Python, but they can be written in any interpreted programming language (shell, Ruby, Python, etc.).Playbooks
| Ansible playbooks | are files written in YAML syntax that define the modules, with arguments, to apply to managed nodes. They declare the tasks that need to be performed. 
| Connection plugins | Plugins that enable communication with managed hosts or cloud providers. These include native SSH, paramiko SSH, and local.


## Ansible Is Python
Ansible is written in Python, and most components that are used in Ansible are written in Python as well. The default Ansible version that is installed on Red Hat Enterprise Linux 7 is based on Python 2.7; the Ansible
release that is used in RHEL 8 is based on Python 3.6. There is no direct relation between an Ansible version and a Python version. Recent versions of Ansible can call either Python 2.x or Python 3.x scripts, but Python
3.x is the better option nowadays because Python 2 is past its end of support life.

## UNDERSTANDING CONTROLLER HOST REQUIREMENTS
* Python 3.x
* An SSH client
* Access to an Ansible Repository
* A dedicated user account that is configured with SSH and sudo permissions
  
Ansible is written in Python, and as a result you have to install Python on the Ansible controller node as well as the Ansible managed nodes.
--------------------------------------
# Installing Ansible on RHEL 8/9
1. On the RHEL 8/9 control node, open a root shell and type subscription-manager repos --list. This shows you a list of currently configured repositories. You should see the standard RHEL 8 repositories.
2. Type subscription-manager repos --enable=ansible-2-for-rhel-8-x86_64-rpms to add the Ansible 2.x repository.
3. Use yum install ansible to install the Ansible software.
4. Use ansible --version to verify that the Ansible software has been installed.
5. Type rpm -qa | grep python to verify that Python 3 is also installed.
--------------------------------------------------------
# Installing on CentOS 8
Method 1: Install Ansible on CentOS 8 / RHEL 8 from EPEL <br>
### sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm <br>
### sudo dnf install  --enablerepo epel-playground  ansible
### ansible --version
# install Ansible on Ubuntu 22.04
### sudo apt update 
### sudo apt-add-repository ppa: ansible/ansible
### sudo apt install ansible
### sudo add-apt-repository --yes --update ppa:ansible/ansible
### ansible --version
---------------------------------------------------

## CONFIGURING MANAGED HOSTS
1. systemctl status sshd
2. rpm -qa | grep python (to see if it’s installed or not, if not install python module)
3. firewall-cmd --list-all (to check ssh is opened or not)
   ## CONFIGURING THE ANSIBLE USER Run the following against all machines
1. Useradd ansible ; echo password | passwd –stdin ansible
2. Visudo >> then add the following entry after line begin with root
       <br> ansible ALL=(ALL) NOPASSWD: ALL

## Run the following against Controller
Su – ansible  <br>
<< Ssh-keygen >>       with default values press enter  <br>
Ssh-copy-id node1.example.com <br>
Ssh-copy-id node2.example.com  <br>

<< TEST>>          ssh node1.example.com no password required   <br>


## Create directory into ansible user home folder
Mkdir project <br>
Touch project/ansible.cfg <br>
Touch project/inventory <br>

Vim project/ansible.cfg <br>

[defaults] <br>
remote_user = ansible <br>
inventory = inventory <br>
[privilege_escalation] <br>
become = True <br>
become_method = sudo <br>
become_user = root <br>
become_ask_pass = False <br>

vim project/inventory <br>

[test] <br>
Node1.example.com <br>
Node2.example.com <br>

[web] <br>
Node2.example.com <br>
[dns:children] <br>
Test <br>
Web <br>

$cd project <br>
$ansible-inventory –list <br>
$ansible-inventory –graph <br>
TEST <br>
$ansible test –m ping <br>
