# OpenVM

OpenWSN-ready Virtual Machine Distribution

## install with vagrant

* `git clone https://github.com/openwsn-berkeley/OpenVM.git`
* `cd OpenVM`
* `vagrant up`
* `ls "C:\Users\user\VirtualBox VMs">file`
* `set /p vm_name=<file`
* `vagrant package --base %vm_name% --output openvm.box`
* `vboxmanage export %vm_name% -o openvm.ova  --ovf10`
* `vagrant destroy -f`
* `vagrant box remove box-cutter/ubuntu1404-desktop -f`

## import ova file to vmware

* open vmware player and go to `File->open`
* choose the openvm.ova file and `import`
* choose `Retry` to  relax OVF specification and virtual hardware compliance checks and try the import again
* wait it finish

## remove VBox GuestAddtion
* open a terminal 
* `cd /opt`
* `cd VBoxGuestAddtions-5.0.18` 
* `sudo ./uninstall.sh`
* `sudo reboot`

Note: the VBoxGuestAddtions folder name may different according to the user version. Just change it accordingly.

## install VMware tools (optional)

follow here: https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018377
