
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.provider = 'virtualbox'

  config.vm.define "net1" do | p |
   p.vm.box = 'centos/7'
   p.vm.host_name = "net1"
   
   p.vm.network  "public_network", bridge: "Intel(R) Dual Band Wireless-AC 8260", ip: "10.50.55.5"
   p.vm.network  "private_network", ip: "192.200.21.5"

       p.vm.provider :virtualbox do |res|

          res.customize ["modifyvm", :id, "--cpus", "1"]
          res.customize ["modifyvm", :id, "--memory", "2048"]
       end

  end

  config.vm.define "net2" do | b |
   b.vm.box= 'centos/7'
   b.vm.host_name = "net2"
   
   b.vm.network  "public_network", bridge: "Intel(R) Dual Band Wireless-AC 8260", ip: "10.50.55.10"
   b.vm.network  "private_network", ip: "192.200.21.10"

      b.vm.provider :virtualbox do |res|

        res.customize ["modifyvm", :id, "--cpus", "1"]
        res.customize ["modifyvm", :id, "--memory", "2048"]
      end

  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.install_mode = "pip"
    ansible.pip_install_cmd = "curl -s https://bootstrap.pypa.io/pip/2.7/get-pip.py | sudo python"
    ansible.version = "2.2.1.0"
  end

end
