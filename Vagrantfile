domain = 'kube'
servers = [
    {
        :name => "jhub-master",
        :type => "master",
        :id => "10",
        :box => "centos/7",
        :eth1 => "10.0.0.10",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "jhub-node1",
        :type => "node",
        :id => "11",
        :box => "centos/7",
        :eth1 => "10.0.0.11",
        :mem => "2048",
        :cpu => "2"
    },
    {
        :name => "jhub-node2",
        :type => "node",
        :id => "12",
        :box => "centos/7",
        :eth1 => "10.0.0.12",
        :mem => "2048",
        :cpu => "2"
    }
]

$script = <<SCRIPT
sudo mv hosts /etc/hosts
SCRIPT


Vagrant.configure("2") do |config|

    servers.each do |opts|
        config.vm.define opts[:name] do |config|

            config.vm.box = opts[:box]
            config.vm.hostname = opts[:name]
            config.vm.network :private_network, ip: opts[:eth1], virtualbox__intnet: domain

            config.vm.provider "virtualbox" do |vb|
                vb.name = opts[:name] + "." + domain
                vb.customize ["modifyvm", :id, "--groups", "/JupyterHub"]
                vb.customize ["modifyvm", :id, "--memory", opts[:mem]]
                vb.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
                vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
                vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
                vb.customize ['modifyvm', :id, '--macaddress1', "5CA1AB1E00"+opts[:id]]
                #vb.customize ['modifyvm', :id, '--natnet1', "192.168/16"]
            end
            config.vm.provision "file", source: "hosts", destination: "hosts"
            config.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
            config.vm.provision "shell", inline: $script

            if opts[:name] == "jhub-master"
               config.vm.network  "forwarded_port", guest: 6443, host: 6443
               config.vm.network  "forwarded_port", guest: 8001, host: 8001
            end
        end
    end
end 
