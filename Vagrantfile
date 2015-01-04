# -*- mode: ruby -*-
# vi: set ft=ruby :

# .envの情報を読み込み
Dotenv.load

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ec2"
  config.vm.synced_folder "./", "/vagrant"

  # AWS settings
  config.vm.provider :aws do |aws, override|
    # アクセスの設定
    override.ssh.username         = ENV['AWS_SSH_USERNAME']
    override.ssh.private_key_path = ENV['AWS_SSH_KEY']
    aws.access_key_id             = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key         = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name              = ENV['AWS_KEYPAIR_NAME']
    aws.security_groups           = [ENV['AWS_SECURITY_GROUP']]
    #aws.session_token            = "SESSION TOKEN"

    # インスタンスの設定
    # amiはEC2 Management Consoleで事前に作成しておく
    aws.region                 = "ap-northeast-1"
    aws.availability_zone      = "ap-northeast-1a"
    aws.instance_type          = "t2.micro"
    aws.instance_ready_timeout = 120
    aws.terminate_on_shutdown  = false
    aws.ami = "ami-90948091"
    aws.tags = {
      "Name"        => "vagrant-aws",
      "Description" => "vagrant-awsを使用して作成されたEC2インスタンスです。",
    }

    # sudoをttyでログインしているユーザからも実行できるようにする
    aws.user_data = <<-USER_DATA
    #!/bin/sh
    echo "Defaults    !requiretty" > /etc/sudoers.d/vagrant-init
    chmod 440 /etc/sudoers.d/vagrant-init
    USER_DATA
  end
end
