def set_vbox(vb, config)
  vb.gui = false
  vb.memory = 768
  vb.cpus = 1
end

Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  # 设置虚拟机的 Box
  config.vm.box = "centos/7"
  (1..6).each do |i|
    name = (i <= 3) ? "kube-m" : "kube-n"
    id = (i <= 3) ? i : i - 3
    config.vm.define "#{name}#{id}" do |n|
      n.vm.hostname = "#{name}#{id}"
      ip_addr = "192.168.56.#{i + 10}"
      n.vm.network :private_network, ip: "#{ip_addr}", auto_config: true
      n.vm.provider :virtualbox do |vb, override|
        vb.name = "#{n.vm.hostname}"
        set_vbox(vb, override)
      end
    end
  end
  # 使用shell脚本进行软件安装和配置
  config.vm.provision "shell", path: "./setup.sh"
end
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkzMTE4MTk1MF19
-->