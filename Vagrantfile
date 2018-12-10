replicado_disk = '~/VirtualBox VMs/replicado_disk.vdi'

Vagrant.configure("2") do |config|
  config.vm.define "replicado" do |host| 
    host.vm.box = "debian/stretch64"
    host.vm.network :private_network, ip: "192.168.8.14"
    config.ssh.insert_key = false
    host.vm.provider :virtualbox do |v|
      v.name = "replicado"
      v.memory = 1024
      v.cpus = 2
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ['createhd', '--filename', replicado_disk, '--size', 50 * 1024]
      v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', replicado_disk]
    end
  end
end
