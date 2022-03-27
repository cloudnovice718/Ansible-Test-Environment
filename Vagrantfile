
# Location where disk file will be stored
disk1 = 'tmp/disk01.vmdk'
disk2 = 'tmp/disk02.vmdk'
disk3 = 'tmp/disk03.vmdk'

# Setting environmental variables
VAGRANT_EXPERIMENTAL="disks"
VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

# Basic configuration settings for managed nodes
  config.vm.define "node1" do |node1|
    node1.vm.box = "generic/fedora35"
    node1.vm.network "private_network", ip: "192.168.56.3"
    node1.vm.synced_folder ".", "/vagrant"

# Creating a second 5GB hard drive with VBoxManage parameters    
    node1.vm.provider "virtualbox" do |node1|
    unless File.exist?(disk1)
      node1.customize [
        'createmedium', 
        '--filename', disk1, 
        '--size', 5 * 1024
      ]
      node1.customize [
        'storagectl', :id, 
        '--name', 'SATA Controller', 
        '--add', 'sata', 
        '--portcount', 2
      ]
      node1.customize [
        'storageattach', :id,  
        '--storagectl', 'SATA Controller', 
        '--port', 1, 
        '--device', 0, 
        '--type', 'hdd', 
        '--medium', disk1
      ]

      end
    end

# Enabling SSH password authentication
    node1.vm.provision "shell", inline: <<-SHELL 
      sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config 
      sudo systemctl restart sshd 

    SHELL

  end

# Basic configuration settings for managed nodes
  config.vm.define "node2" do |node2|
    node2.vm.box = "generic/fedora35"
    node2.vm.network "private_network", ip: "192.168.56.4"
    node2.vm.synced_folder ".", "/vagrant"

# Creating a second 5GB hard drive with VBoxManage parameters
    node2.vm.provider "virtualbox" do |node2|
      unless File.exist?(disk2)
        node2.customize [
          'createmedium', 
          '--filename', disk2, 
          '--format', 'vmdk', 
          '--size', 5 * 1024
        ]

        node2.customize [
          'storagectl', :id, 
          '--name', 'SATA Controller', 
          '--add', 'sata', 
          '--portcount', 2
        ]

        node2.customize [
          'storageattach', :id,  
          '--storagectl', 'SATA Controller', 
          '--port', 1, 
          '--device', 0, 
          '--type', 'hdd', 
          '--medium', disk2
        ]

      end
    end

# Enabling SSH password authentication
    node2.vm.provision "shell", inline: <<-SHELL 
      sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config 
      sudo systemctl restart sshd 

    SHELL

  end  

# Basic configuration settings for managed nodes
  config.vm.define "node3" do |node3|
    node3.vm.box = "generic/fedora35"
    node3.vm.network "private_network", ip: "192.168.56.5"
    node3.vm.synced_folder ".", "/vagrant"

# Customizing VM guest hardware settings for node3
    node3.vm.provider "virtualbox" do |node3|
      node3.memory = "512"

# Creating a second 5GB hard drive with VBoxManage parameters      
      unless File.exist?(disk3)
        node3.customize [
          'createmedium', 
          '--filename', disk3, 
          '--format', 'vmdk', 
          '--size', 5 * 1024
        ]

        node3.customize [
          'storagectl', :id, 
          '--name', 'SATA Controller', 
          '--add', 'sata', 
          '--portcount', 2
        ]

        node3.customize [
          'storageattach', :id,  
          '--storagectl', 'SATA Controller', 
          '--port', 1, 
          '--device', 0, 
          '--type', 'hdd', 
          '--medium', disk3
        ]
    
      end
    end

# Enabling SSH password authentication
    node3.vm.provision "shell", inline: <<-SHELL 
      sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config 
      sudo systemctl restart sshd

    SHELL

  end

# Configuration settings for the Ansible control node. 
  config.vm.define "control" do |control|
    control.vm.box = "generic/fedora35"
    control.vm.network "private_network", ip: "192.168.56.2" 
    control.vm.synced_folder ".", "/vagrant"
    control.vm.provider "virtualbox" do |control|

  end    

# Some Linux packages will be installed upon VM creation
    control.vm.provision "shell", inline: <<-SHELL 
      sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config 
      sudo systemctl restart sshd
      sudo yum update -y
      sudo yum install sshpass -y
      sudo yum install ansible -y

    SHELL


# Using built-in ansible_local module to provide default file locations
    control.vm.provision :ansible_local do |ansible|
      ansible.install = false
      ansible.inventory_path = "/vagrant/inventory.ini"
      ansible.playbook = "/vagrant/setup.yml"           # Ansible playbook that will complete the remaining VM guest configurations
      ansible.config_file = "/vagrant/ansible.cfg"
      ansible.limit = "all"
      ansible.compatibility_mode = "auto"
  end

end
end

