# Ansible-Test-Environment
Virtual machine to practice Ansible automation

This is a re-upload of my VM deployment project that I created using Vagrant. Changes made include replacing CentOS 8 boxes with Fedora version 35, which provides a working yum repository (CentOS has since taken theirs offline into 2022) and the latest Python 3.10 release (current Ansible 2.11 will only support Python 3.8+). When fully provisioned, there will four total VMs (One Ansible control node and three managed nodes) preconfigured for the purposes of practicing Ansible automation in a sandbox environment. Recommended for those who are looking to practice their Ansible skills.

Tested on Mac OS Monterrey and Windows 11.

# Prerequisites:
Vagrant (https://www.vagrantup.com/downloads)

Virtualbox (https://www.virtualbox.org/wiki/Downloads)

Virtualbox Extension Pack (https://www.virtualbox.org/wiki/Downloads)

# Once all components outlined above are installed:
Click the green button labeled “Code” at the top of the page

Select “Download ZIP”

Download and extract the contents to a folder of your choosing.

Open a command line interface as administrator (i.e. Powershell, Terminal, iTerm2, etc.)

Browse to the directory that contains the unzipped contents

Run the 'vagrant up' (no quotes) command to begin the provisioning process

Once the provisioning is complete, you may access the VM environment by entering 'vagrant ssh control' then switch to the 'ansible' user by typing 'su - ansible' followed by the provided password.

