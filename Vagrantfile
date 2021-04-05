# -*- mode: ruby -*-
# vi: set ft=ruby :
# the board parameter needs to be supplied before the vagrant command like this: vagrant --board=x230 up instead of the more intuitive vagrant up --board=x230
require 'getoptlong'

opts = GetoptLong.new(
[ '--board', GetoptLong::OPTIONAL_ARGUMENT ]
)
board='x230'
opts.ordering=(GetoptLong::REQUIRE_ORDER)
opts.each do |opt, arg|
	case opt
	when '--board'
		board = arg
	end
end
$name = "heads-build"
$cpus = 2
$mem = 1024
$gui = false
$script = <<-SCRIPT
apt update > /dev/null 2>&1
git clone https://github.com/osresearch/heads
cd heads
grep 'apt install' .circleci/config.yml|sh
make BOARD="$1"
cp build/"$1"/*.rom /vagrant
SCRIPT

Vagrant.configure("2") do |config|
  #sshfs plugin is necessary because virtualbox guest additions preinstalled in debian base box are too old to use standard mount
  config.vagrant.plugins = "vagrant-sshfs"
  config.vm.box = "generic/debian10"
  config.vm.hostname = $name
  config.vm.synced_folder ".", "/vagrant", type: "sshfs"
  #additional resources will not provide benefit since heads build scripts are configured to use a single thread
  config.vm.provider "virtualbox" do |vb|
    vb.gui = $gui
    vb.name = $name
    vb.memory = $mem
    vb.cpus = $cpus
  end
  config.vm.provider "hyperv" do |h|
    h.vmname = $name
    h.cpus = $cpus
    h.maxmemory = $mem
 end
 config.vm.provider "libvirt" do |l|
   l.title = $name
   l.memory = $mem
   l.cpus = $cpus
 end
 config.vm.provision "shell" do |s|
   s.inline = $script
   s.args = "#{board}"
 end
  config.vm.post_up_message = "Build complete, you can now shutdown the VM using vagrant halt command"
  config.vm.post_up_message = "Rom file should be present in the directory where the Vagrantfile is stored"
end
