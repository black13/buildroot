RELEASE='2017.05'

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
    vb.memory = "1524"
end
  
  config.vm.provision 'shell' do |s|
	s.inline = 'echo Setting up machine name'

	config.vm.provider :vmware_fusion do |v, override|
	   v.vmx['displayname'] = "Buildroot #{RELEASE}"
	end

	config.vm.provider :virtualbox do |v, override|
		required_plugins = %w( vagrant-vbguest )
		required_plugins.each do |plugin|
		  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
		end
	end
  end

	config.vm.provision 'shell' do |s|
		s.inline = 'echo Setting up machine name'

		config.vm.provider :vmware_fusion do |v, override|
			v.vmx['displayname'] = "Buildroot #{RELEASE}"
		end

		config.vm.provider :virtualbox do |v, override|
			v.name = "Buildroot #{RELEASE}"
		end
	end

	config.vm.provision 'shell', privileged: true, inline:
		"sed -i 's|deb http://us.archive.ubuntu.com/ubuntu/|deb mirror://mirrors.ubuntu.com/mirrors.txt|g' /etc/apt/sources.list
		dpkg --add-architecture i386
		apt-get -q update
		apt-get purge -q -y snapd lxcfs lxd ubuntu-core-launcher snap-confine
		apt-get -q -y install build-essential libncurses5-dev \
			git bzr cvs mercurial subversion libc6:i386 unzip bc
		apt-get -q -y autoremove
		apt-get -q -y clean
		update-locale LC_ALL=C"

	config.vm.provision 'shell', privileged: false, inline:
		"echo 'Downloading and extracting buildroot #{RELEASE}'
		wget -q -c http://buildroot.org/downloads/buildroot-#{RELEASE}.tar.gz
		tar axf buildroot-#{RELEASE}.tar.gz"

end
