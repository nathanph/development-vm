# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	# All Vagrant configuration is done here. The most common configuration
	# options are documented and commented below. For a complete reference,
	# please see the online documentation at vagrantup.com.

	# Every Vagrant virtual environment requires a box to build off of.
	config.vm.box = "ubuntu/trusty64"

	# Disable automatic box update checking. If you disable this, then
	# boxes will only be checked for updates when the user runs
	# `vagrant box outdated`. This is not recommended.
	# config.vm.box_check_update = false

	# Create a forwarded port mapping which allows access to a specific port
	# within the machine from a port on the host machine. In the example below,
	# accessing "localhost:8080" will access port 80 on the guest machine.
	# config.vm.network "forwarded_port", guest: 80, host: 8080

	# Create a private network, which allows host-only access to the machine
	# using a specific IP.
	# config.vm.network "private_network", ip: "192.168.33.10"

	# Create a public network, which generally matched to bridged network.
	# Bridged networks make the machine appear as another physical device on
	# your network.
	# config.vm.network "public_network"

	# If true, then any SSH connections made will enable agent forwarding.
	# Default value: false
	# config.ssh.forward_agent = true

	# Share an additional folder to the guest VM. The first argument is
	# the path on the host to the actual folder. The second argument is
	# the path on the guest to mount the folder. And the optional third
	# argument is a set of non-required options.
	# config.vm.synced_folder "../data", "/vagrant_data"

	# Provider-specific configuration so you can fine-tune various
	# backing providers for Vagrant. These expose provider-specific options.
	# Example for VirtualBox:
	#
	# config.vm.provider "virtualbox" do |vb|
	#   # Don't boot with headless mode
	#   vb.gui = true
	#
	#   # Use VBoxManage to customize the VM. For example to change memory:
	#   vb.customize ["modifyvm", :id, "--memory", "1024"]
	# end
	#
	# View the documentation for the provider you're using for more
	# information on available options.

	# Enable provisioning with CFEngine. CFEngine Community packages are
	# automatically installed. For example, configure the host as a
	# policy server and optionally a policy file to run:
	#
	# config.vm.provision "cfengine" do |cf|
	#   cf.am_policy_hub = true
	#   # cf.run_file = "motd.cf"
	# end
	#
	# You can also configure and bootstrap a client to an existing
	# policy server:
	#
	# config.vm.provision "cfengine" do |cf|
	#   cf.policy_server_address = "10.0.2.15"
	# end

	# Enable provisioning with Puppet stand alone.  Puppet manifests
	# are contained in a directory path relative to this Vagrantfile.
	# You will need to create the manifests directory and a manifest in
	# the file default.pp in the manifests_path directory.
	#
	# config.vm.provision "puppet" do |puppet|
	#   puppet.manifests_path = "manifests"
	#   puppet.manifest_file  = "site.pp"
	# end

	# Enable provisioning with chef solo, specifying a cookbooks path, roles
	# path, and data_bags path (all relative to this Vagrantfile), and adding
	# some recipes and/or roles.
	#
	config.vm.provision "chef_solo" do |chef|

		chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
		chef.roles_path = "roles"
		chef.data_bags_path = "data_bags"
		chef.node_name = "node_name"

		chef.add_recipe "apt"
		chef.add_recipe "git"
		chef.add_recipe "vim"
		chef.add_recipe "python"
		chef.add_recipe "nodejs"
		chef.add_recipe "build-essential"
		chef.add_recipe "java"
		chef.add_recipe "vim_config"
		chef.add_recipe "golang"

		chef.json = {
			"go" => {
				"version" => "1.4.2"
			},
			"vim_config" => {
				"owner" => "vagrant",
				"owner_group" => "vagrant",
				"bundle_dir" => "/home/vagrant/.vim/bundle",
						"plugin_manager" => "vundle",
						"config_file_mode" => "remote_file",
				"config_file_path" => "/home/vagrant/.vimrc",
						"remote_config_url" => "https://raw.githubusercontent.com/nathanph/dotfiles/master/.vimrc"
			}
		}

	end

	config.vm.provision "chef_apply" do |chef|

		chef.recipe = <<-RECIPE
			directories = [ "/home/vagrant/.vim/.undo",
							"/home/vagrant/.vim/.backup",
							"/home/vagrant/.vim/.swp" ]
			directories.each do |path|
				directory path do
					owner	"vagrant"
					group	"vagrant"
					mode	"0700"
					action	"create"
				end
			end

			execute "set git user.name" do
				user		"vagrant"
				environment	({'HOME' => '/home/vagrant'})
				command		"git config --global user.name 'Nathan Hernandez'"
				action		"run"
			end

			execute "set git user.email" do
				user		"vagrant"
				environment	({'HOME' => '/home/vagrant'})
				command		"git config --global user.email 'email@nathanph.com'"
				action		"run"
			end

            execute "set git matching simple" do
                user        "vagrant"
                environment ({'HOME' => '/home/vagrant'})
                command     "git config --global push.default simple"
                action      "run"
            end

			execute "install vundle plugins" do
				user		"vagrant"
				environment	({'HOME' => '/home/vagrant'})
				command		"yes | vim +PluginInstall +qall"
				action		"run"
			end
		RECIPE

	end

	# Enable provisioning with chef server, specifying the chef server URL,
	# and the path to the validation key (relative to this Vagrantfile).
	#
	# The Opscode Platform uses HTTPS. Substitute your organization for
	# ORGNAME in the URL and validation key.
	#
	# If you have your own Chef Server, use the appropriate URL, which may be
	# HTTP instead of HTTPS depending on your configuration. Also change the
	# validation key to validation.pem.
	#
	# config.vm.provision "chef_client" do |chef|
	#   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
	#   chef.validation_key_path = "ORGNAME-validator.pem"
	# end
	#
	# If you're using the Opscode platform, your validator client is
	# ORGNAME-validator, replacing ORGNAME with your organization name.
	#
	# If you have your own Chef Server, the default validation client name is
	# chef-validator, unless you changed the configuration.
	#
	#   chef.validation_client_name = "ORGNAME-validator"
end
