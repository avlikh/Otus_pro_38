Vagrant.configure("2") do |config|
  # Указываем параметры виртуальных машин
  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 1
  end

  # Указываем имена хостов, IP-адреса и проброс портов
  boxes = [
    { :box_name => "generic/centos8",
      :name => "ipa.otus.lan",
      :ip => "192.168.57.10",
      :forward_port => { host: 443, guest: 443 } # Проброс порта 443
    },
    { :box_name => "centos/stream9",
      :name => "client1.otus.lan",
      :ip => "192.168.57.11"
    },
    { :box_name => "centos/stream9",
      :name => "client2.otus.lan",
      :ip => "192.168.57.12"
    }
  ]

  # Цикл для настройки каждой виртуальной машины
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.box = opts[:box_name]
      config.vm.hostname = opts[:name]
      config.vm.network "private_network", ip: opts[:ip]

      # Настройка проброса порта (если он указан)
      if opts[:forward_port]
        config.vm.network "forwarded_port",
                          guest: opts[:forward_port][:guest],
                          host: opts[:forward_port][:host]
      end

      # Запуск Ansible playbook ТОЛЬКО для client2.otus.lan
      if opts[:name] == "client2.otus.lan"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "provision.yml"  # Путь к Playbook
          ansible.config_file = "ansible.cfg" # Файл конфигурации Ansible
          ansible.become = true               # Использование sudo
          ansible.limit = "all"               # Выполнять на всех узлах
        end
      end
    end
  end
end
