#Define the list of machines
slurm_cluster = {
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
    slurm_cluster.each_pair do |name, options|
        global_config.vm.define name do |config|
            #VM configurations
            config.vm.box = "skinoshita/scientific-6.5"
            config.vm.hostname = "#{name}"
            config.vm.network :private_network, ip: options[:ipaddress]

            #VM specifications
            config.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--memory", "512"]
            end

            #VM provisioning
            config.vm.provision :shell,
                :inline => $script
        end
    end
end
