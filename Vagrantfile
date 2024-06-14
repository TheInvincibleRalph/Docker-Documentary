# This block starts the Vagrant configuration and
# specifies the configuration version. "2" refers to Vagrant 1.1 and above.
Vagrant.configure("2") do |config|

    # Define the first VM
    config.vm.define "web" do |web|
      web.vm.box = "ubuntu/bionic64"
      
      # Network settings
      web.vm.network "private_network", ip: "192.168.33.10"
      web.vm.network "forwarded_port", guest: 80, host: 8080
  
      # Synced folder
      web.vm.synced_folder "../data", "/vagrant_data"
  
      # Provider settings
      web.vm.provider "virtualbox" do |vb|
        vb.name = "web-server"
        vb.memory = "2048"
        vb.cpus = 2
      end
  
      # Provision with a shell script
      web.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
      SHELL
  
      # Provision with Ansible
      web.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
      end
    end
  
    # Define the second VM
    config.vm.define "db" do |db|
      db.vm.box = "ubuntu/bionic64"
      
      # Network settings
      db.vm.network "private_network", ip: "192.168.33.11"
  
      # Provider settings
      db.vm.provider "virtualbox" do |vb|
        vb.name = "db-server"
        vb.memory = "1024"
        vb.cpus = 1
      end
  
      # Provision with a shell script
      db.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y mysql-server
        sudo mysql_secure_installation
      SHELL
    end
  
    # Global settings
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
    end
  
    config.vm.provision "shell", inline: <<-SHELL
      echo "This is a global provisioner that runs on all VMs."
    SHELL
  
  end
  