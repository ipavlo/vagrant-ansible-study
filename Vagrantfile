VAGRANTFILE_API_VERSION = "2"
Vagrant.configure("2") do |config|
  N = 3

  VAGRANT_VM_PROVIDER = "virtualbox"
  ANSIBLE_RAW_SSH_ARGS = []

  (1..N-1).each do |machine_id|
    ANSIBLE_RAW_SSH_ARGS << "-o IdentityFile=.vagrant/machines/machine#{machine_id}/#{VAGRANT_VM_PROVIDER}/private_key"
  end

    config.ssh.insert_key = false
  (1..N).each do |machine_id|
    config.vm.define "machine#{machine_id}" do |machine|
      machine.vm.box = "ubuntu/trusty64"
      machine.vm.hostname = "machine#{machine_id}"
      machine.vm.network "private_network", ip: "192.168.77.#{10+machine_id-1}"

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == N
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "main.yml"
          ansible.inventory_path = "staging"
          ansible.verbose = "-vvvv"
           ansible.sudo = true
          ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
        end
      end
    end
  end
end				

