
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Base VM OS configuration.
  config.vm.box = "centos/7"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.hostname = "kafka"

  config.vm.provision :shell do |shell|
    shell.inline = "sudo yum install -y epel-release; sudo yum update -y"
  end

  N = 3
  (1..N).each do |machine_id|
    config.vm.define "kafka-#{machine_id}" do |machine|
      machine.vm.hostname = "kafka-#{machine_id}"
      machine.vm.network :private_network, ip: "192.168.23.#{10+machine_id}"

      # Virtualbox configuration
      config.vm.provider :virtualbox do |virtualbox|
        virtualbox.name = "kafka-#{machine_id}"
        virtualbox.customize ['modifyvm', :id, '--memory', '2048']
        virtualbox.customize ['modifyvm', :id, '--ioapic', 'on']
        virtualbox.customize ['modifyvm', :id, '--cpus', '1']
        virtualbox.customize ['modifyvm', :id, '--cpuexecutioncap', '50']
        virtualbox.customize ['modifyvm', :id, '--groups', '/kafka']
      end

      if machine_id == 1
        machine.vm.network :forwarded_port, id: 'kafka', guest: 9092, host: 9092
      end

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == N
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "tests/test.yml"
          ansible.groups = {
            :kafka_hosts => (1..N).map { |element| "kafka-#{element}" }
          }
          ansible.host_vars = {
              :all => {
                  :zookeeper_hosts => (1..N).map { |element| "kafka-#{element}" }
              },
              'kafka-1' => {
                  :zookeeper_config_id => 1,
                  :kafka_config_broker_id => 1
              },
              'kafka-2' => {
                  :zookeeper_config_id => 2,
                  :kafka_config_broker_id => 2
              },
              'kafka-3' => {
                  :zookeeper_config_id => 3,
                  :kafka_config_broker_id => 3
              }
          }
          ansible.extra_vars = {
              web_interface: 'eth0',
              cluster_interface: 'eth1',
              kafka_quorum: %w(192.168.23.11 192.168.23.12 192.168.23.13)
          }
        end
      end
    end
  end

end
