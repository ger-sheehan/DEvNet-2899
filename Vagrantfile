# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "DevNet-2899"
  config.vm.define "bird1" do |bird1|
    bird1.vm.host_name = "bird1"
    bird1.vm.network "private_network",
                         ip: "10.10.10.11",
                         virtualbox__intnet: "bird1-to-n9kv1"
    bird1.vm.network "private_network",
                         ip: "1.1.1.1",
                         virtualbox__intnet: "Network to Advertise"
    #bird1.vm.synced_folder "playbooks/", "/playbooks/"
    bird1.vm.provision "ansible" do |ansible|
      ansible.playbook = "bird1.yml"
    bird1.vm.provider :virtualbox do |vb|
            # Set the timesync threshold to 10 seconds, instead of the default 20 minutes.
        vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
        end
    end
  end
  config.vm.box = "DevNet-2899"
  config.vm.define "bird2" do |bird2|
    bird2.vm.host_name = "bird2"
    bird2.vm.network "private_network",
                         ip: "20.20.20.11",
                         virtualbox__intnet: "bird2-to-n9kv1"
    bird2.vm.network "private_network",
                        ip: "30.30.30.11",
                        virtualbox__intnet: "ES-to-n9kv1"
    bird2.vm.network "private_network",
                         ip: "2.2.2.2",
                         virtualbox__intnet: "Network to Advertise"
    bird2.vm.network :forwarded_port, guest: 5601, host: 5601
    bird2.vm.network :forwarded_port, guest: 9200, host: 9200
    bird2.vm.network :forwarded_port, guest: 80, host: 80

    bird2.vm.provision "ansible" do |ansible|
      ansible.playbook = "bird2.yml"
    bird2.vm.provider :virtualbox do |vb|
          # Set the timesync threshold to 10 seconds, instead of the default 20 minutes.
          vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
      end
    end
  end

  config.vm.define "n9kv1" do |n9kv1|

        n9kv1.vm.box = "DevNet-3618"

        n9kv1.ssh.insert_key = false

        n9kv1.vm.boot_timeout = 240

        n9kv1.vm.synced_folder '.', '/vagrant', disabled: true

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "bird1-to-n9kv1"

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "bird2-to-n9kv1"

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "ES-to-n9kv1"

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "nxosv_network4"

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "nxosv_network5"

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "nxosv_network6"

        n9kv1.vm.network "private_network", auto_config: false, virtualbox__intnet: "nxosv_network7"

        n9kv1.vm.provider :virtualbox do |vb|

                vb.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 1000 ]

        end
      end
end
