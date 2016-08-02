# OpenVM

OpenWSN-ready Virtual Machine Distribution

## Install with vagrant

* `git clone https://github.com/openwsn-berkeley/OpenVM.git`
* `cd OpenVM`
* `vagrant up`
* `ls "C:\Users\user\VirtualBox VMs">file`
* `set /p vm_name=<file`
* `vagrant package --base %vm_name% --output openvm.box`
* `vboxmanage export %vm_name% -o openvm.ova  --ovf10`
* `vagrant destroy -f`
* `vagrant box remove box-cutter/ubuntu1404-desktop -f`

## Import ova file to vmware

* open vmware player and go to `File->open`
* choose the openvm.ova file and `import`
* choose `Retry` to  relax OVF specification and virtual hardware compliance checks and try the import again
* wait it finish

## Remove VBox GuestAddtion
* open a terminal 
* `cd /opt`
* `cd VBoxGuestAddtions-5.0.18` 
* `sudo ./uninstall.sh`
* `sudo reboot`

Note: the VBoxGuestAddtions folder name may different according to the user version. Just change it accordingly.

## Install VMware Tools (optional)

follow here: https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1018377.
After 

## Shooting Board

On a 64-bit version machine,  if the vagrant is run as administrator, the box downloaded through vagrant locates at `C:\Windows\SysWOW64\config\systemprofile\.vagrant.d`. However when to execute the vagrant to bring the virtual machine up, vagrant will look for the box at `C:\Windows\System32\config\systemprofile\.vagrant.d` since vagrant is a 32 bit program. Then you may have following error. 

> Command: ["import", "-n", "C:\\Windows\\System32\\config\\systemprofile\\.vagrant.d\\boxes\\box-cutter-VAGRANTSLASH-ubuntu1404-desktop\\2.0.18\\virtualbox\\box.ovf"]

> Stderr: 0%...

> Progress state: VBOX_E_FILE_ERROR

> VBoxManage.exe: error: Appliance read failed

> VBoxManage.exe: error: Could not read OVF file 'box.ovf' (VERR_PATH_NOT_FOUND)
> 
> VBoxManage.exe: error: Details: code VBOX_E_FILE_ERROR (0x80bb0004), component ApplianceWrap, interface IAppliance
> 
> VBoxManage.exe: error: Context: "enum RTEXITCODE __cdecl handleImportAppliance(struct HandlerArg *)" at line 307 of file VBoxManageAppliance.cpp

To solve this problem, we need to tell vagrant where to look for the box by modifying the **environment.rb** file locating at `C:\HashiCorp\Vagrant\embedded\gems\gems\vagrant-1.8.1\lib\vagrant`.

Try to find some context like: 

      # Setup the home directory
      @home_path  ||= Vagrant.user_data_path
      @home_path  = Util::Platform.fs_real_path(@home_path)
      @boxes_path = @home_path.join("boxes")
      @data_dir   = @home_path.join("data")
      @gems_path  = @home_path.join("gems")
      @tmp_path   = @home_path.join("tmp")
      @machine_index_dir = @data_dir.join("machine-index")

and modify the second line by 

      @home_path  = Util::Platform.fs_real_path("C:\\Windows\\SysWOW64\\config\\systemprofile\\.vagrant.d")


*Note*: There is no such issue when running vagrant as a regular user.