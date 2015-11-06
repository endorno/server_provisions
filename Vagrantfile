# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

def aws_config_setting(config)
  config.vm.box = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
end

def aws_common_setting(aws, override)
    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name = ENV['AWS_KEYPAIR_NAME']

    aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 10 }]
    aws.security_groups = ["computational_resource"]

    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = ENV['AWS_KEY_PEM']
end


Vagrant.configure(2) do |config|
  config.vm.define :local do |local|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "site.yml"
    end
  end

  config.vm.define :cpu do |cpu|
    aws_config_setting(config)
    cpu.vm.provider :aws do |aws, override|
      aws_common_setting(aws, override)

      aws.region = "ap-northeast-1"
      aws.ami = "ami-936d9d93" #default ubuntu hmv

      aws.instance_type = "t2.micro"
    end

    cpu.vm.provision "ansible" do |ansible|
      ansible.extra_vars = {
        "user_name" => "ubuntu",
      }
      ansible.playbook = "site.yml"
    end
  end

  config.vm.define :gpu do |gpu|
    aws_config_setting(config)
    gpu.vm.provider :aws do |aws, override|
      aws_common_setting(aws, override)

      aws.region = "ap-northeast-1"
      aws.ami = "ami-5ac1e634" # cuda installed AMI
      # aws.ami = "ami-936d9d93" #default ubuntu hmv

      aws.instance_type = "g2.2xlarge"
    end
  end
end
