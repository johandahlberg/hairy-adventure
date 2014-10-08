#Define the list of machines
machines = {
    :biotank10 => {
        :hostname => "biotank10",
        :ipaddress => "10.10.10.50",
    },
    :biotank11 => {                                                              
        :hostname => "biotank11",
        :ipaddress => "10.10.10.51",
    },
}

#Provisioning inline script
$script = <<SCRIPT
echo "10.10.10.50    biotank10" >> /etc/hosts
echo "10.10.10.51    biotank11" >> /etc/hosts
SCRIPT

Vagrant.configure("2") do |global_config|
  
    machines.each_pair do |name, options|
        global_config.vm.define name do |config|
            #VM configurations
            config.vm.box = "dannycoates/sl64"
            config.vm.hostname = "#{name}"
            config.vm.network :private_network, ip: options[:ipaddress]
  
            #VM specifications
            config.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--memory", "512"]
                # Add a virtual disk to use for data/mirroring
                v.customize ['createhd', '--filename', "./#{name}_data_disk.vmdk", '--format', 'VMDK', '--size', 1024]
                v.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', "./#{name}_data_disk.vmdk"]
            end

            #VM provisioning
            config.vm.provision :shell,
                :inline => $script

        end
    end
end
