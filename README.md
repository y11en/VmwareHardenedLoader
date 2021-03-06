# VmwareHardenedLoader
Vmware Hardened VM detection mitigation loader

For now, only Windows (vista~win10) x64 guests are supported.

It get vmware guest undetected by VMProtect 3.2 (anti-vm feature).

## What it does

the VmLoader driver patches SystemFirmwareTable at runtime, it removes all detectable signatures like "VMware" "Virtual" "VMWARE".

## Warning

Do not install vmtools, it will ruin everything!

use TeamViewer / AnyDesk / mstsc / VNC viewer instead!

## First Step: Add following settings into .vmx

```
hypervisor.cpuid.v0 = "FALSE"
board-id.reflectHost = "TRUE"
hw.model.reflectHost = "TRUE"
serialNumber.reflectHost = "TRUE"
smbios.reflectHost = "TRUE"
isolation.tools.getPtrLocation.disable = "TRUE"
isolation.tools.setPtrLocation.disable = "TRUE"
isolation.tools.setVersion.disable = "TRUE"
isolation.tools.getVersion.disable = "TRUE"
monitor_control.disable_directexec = "TRUE"
monitor_control.disable_chksimd = "TRUE"
monitor_control.disable_ntreloc = "TRUE"
monitor_control.disable_selfmod = "TRUE"
monitor_control.disable_reloc = "TRUE"
monitor_control.disable_btinout = "TRUE"
monitor_control.disable_btmemspace = "TRUE"
monitor_control.disable_btpriv = "TRUE"
monitor_control.disable_btseg = "TRUE"
```

## Second Step: regenerate MAC address for guest

![mac](https://github.com/hzqst/VmwareHardenedLoader/raw/master/img/4.png)

## Second Step: Load vmloader.sys in vm guest
open command prompt as System Administrator, use the following commands

```
sc create vmloader binPath= "\??\c:\vmloader.sys" type= "kernel"
sc start vmloader
```

If an error occurs when start service, use DbgView to see whats happened. you can post an issue with DbgView output information if it does't work on your environment.

If no error, then everything works fine.

you could put "vmloader.sys" wherever you want, except vmware shared folders.

when you no longer need the mitigation, use
```
sc stop vmloader
sc delete vmloader
```
to unload the driver.

## Vmware guest win8.1 x64 with VMProtect 3.2 packed program (anti-vm option enabled)

![before](https://github.com/hzqst/VmwareHardenedLoader/raw/master/img/1.png)
![sigs](https://github.com/hzqst/VmwareHardenedLoader/raw/master/img/2.png)
![after](https://github.com/hzqst/VmwareHardenedLoader/raw/master/img/3.png)
