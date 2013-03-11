# -*- mode: ruby -*-
# vi: set ft=ruby :

$setup = <<SCRIPT
# Update apt
apt-get update

# Install the things we need to build
apt-get install -y autoconf
apt-get install -y automake
apt-get install -y build-essential
apt-get install -y git-core
apt-get install -y libtool
apt-get install -y telnet
apt-get install -y screen

# Compile Erlang from source
if [ ! -f /usr/local/bin/erl ]; then
  pushd /tmp

  # Download Erlang
  wget --progress=dot -e dotbytes=1M http://www.erlang.org/download/otp_src_R15B03-1.tar.gz

  # Untar it
  tar xvzf otp_src_R15B03-1.tar.gz

  # Compile it
  pushd otp_src_R15B03
  ./configure
  make
  make install
  popd

  popd
fi
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.provision :shell, :inline => $setup
  config.vm.network :forwarded_port,
    guest: 22,
    host:  2456,
    id: "ssh"

  config.vm.provider :vmware_fusion do |p|
    p.vmx["numvcpus"] = "2"
    p.vmx["coresPerSocket"] = "2"
    p.vmx["memsize"] = "1024"
  end

  config.vm.define :n1 do |x|
      x.vm.network :private_network, ip: "33.33.36.10"
  end

  config.vm.define :n2 do |y|
      y.vm.network :private_network, ip: "33.33.36.11"
  end

  config.vm.define :n3 do |z|
      z.vm.network :private_network, ip: "33.33.36.12"
  end
end
