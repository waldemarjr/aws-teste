VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider :aws do |aws, override|

    config.vm.provision :shell do |s|
  	s.env = {AWS_ACCESS_KEY:ENV['AWS_ACCESS_KEY']}
  	s.path = 'scripts/aws_settings.sh'
    end

    config.vm.box = "dummy"
    config.vm.host_name = "webserver01"

    # AWS Credentials:
    aws.access_key_id = ENV['AWS_ACCESS_KEY']
    aws.secret_access_key = ENV['AWS_SECRET_KEY']
    aws.keypair_name = ENV['AWS_KEY_PEMNAME']

    # AWS Location:
    aws.region = ENV['AWS_REGION']
    aws.region_config ENV['AWS_REGION'], :ami => ENV['AWS_AMI']
    aws.instance_type = ENV['AWS_TYPE_INS']
    aws.tags = {
      'Name' => ENV['INSTANCE_NAME'],
    }

    # AWS Storage:
    aws.block_device_mapping = [{
      'DeviceName' => ENV['AWS_DEVICE_NAME'],
      'Ebs.VolumeSize' => ENV['AWS_DEVICE_SIZE'],
      'Ebs.DeleteOnTermination' => ENV['AWS_DEVICE_VOL_DEL'],
      'Ebs.VolumeType' => ENV['AWS_DEVICE_VOL_TYPE'],
    }]

    # AWS Network:
    aws.security_groups = ENV['AWS_SEC_GROUP']
    aws.associate_public_ip = ENV['AWS_ASSOC_PUB_IP']
    aws.subnet_id = ENV['AWS_SUBNET_ID']

    # SSH:
    override.ssh.username = ENV['AWS_SSH_USERNAME']
    override.ssh.private_key_path = ENV['AWS_KEY_PEMPATH']

  end

  # Shell
  config.vm.provision "shell" do |shell|
    shell.inline =
            "
              echo 'webserver01' > /etc/hostname
              hostname `cat /etc/hostname`
            "
  end

  # Ansible provisioning:
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.verbose = "false"
  end

end
