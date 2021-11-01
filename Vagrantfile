Vagrant.configure("2") do |config|
  config.vm.box = "openlogic/rockylinux-8"
  config.vm.box_version = "8.3.2011-RC1.v20210503"

  config.vm.define "node1" do |node1|
    node1.vm.network "private_network", ip: "192.168.33.11"
    node1.vm.hostname = "node1"
    node1.vm.provision "shell", inline: "wget https://dl.influxdata.com/telegraf/releases/telegraf-1.13.4-1.x86_64.rpm && mv telegraf-1.13.4-1.x86_64.rpm /tmp"
    node1.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end
end
