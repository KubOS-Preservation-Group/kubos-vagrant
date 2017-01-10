# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty32"
  config.vm.provision "file", source: "./bin/libmsp430.so", destination: "~/libmsp430.so"
  config.vm.provision "file", source: "./bin/iobc_toolchain.tar.gz", destination: "~/iobc_toolchain.tar.gz"
  config.vm.provision "shell", path: "./script/provision.sh"

  # Enable USB access
  usb_devs = [
    # Required for programming new fraunchpad
    ['0x2047', '0x0013', 'MSP4305969 Launchpad programmer'],
    ['0x2047', '0x0203', 'MSP4305969 Launchpad FW updater'],
    # Other USB ids from TI's udev rules.
    ['0x2047', '0x0010', 'MSP430UIF'],
    ['0x2047', '0x0014', 'MSP430UIF'],
    ['0x2047', '0x0204', 'MSP430UIF'],
    # For older fraunchpads and launchpads
    ['0x0451', '0xf432', 'eZ430'],
    # Misc devices
    ['0x15ba', '0x0031', 'Olimex JTAG tiny'],
    #STM Devices
    ['0x0483', '0x3748', 'STM32 STLink'],
    ['0x0483', '0x374b', 'STM32 STLink'],
    ['0x0483', '0xdf11', 'STM32 BOOTLOADER']
  ]
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.customize ['modifyvm', :id, '--usb', 'on']
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
    usb_devs.each do |dev|
      vb.customize ['usbfilter', 'add', '0', '--target', :id, '--vendorid', dev[0], '--productid', dev[1], '--name', dev[2]]
    end
  end
end
