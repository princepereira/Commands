# VM PREREQUISITES #
- RAM: 8000 MB
- CPU Cores: 4
- Use VHDX
- Use Generation 2

# STEPS #
- Copy required VHDX, Scripts and Binaries from `winbuilds releases` directory to the Hyper-V `Host Machine` (Physical DevBox)
- Create Hyper-V `Guest VM` using the VHDX and Enable Hyper-V Virtualization
- Register the VM with Microsoft CorpNet
- Copy the scripts to `Guest VM` using net use command
- Install containerd in `Guest VM` using the script.
- Run RITP Tests

# STEPS IN DETAIL #

## Copy a VHDX image to Host VM from winbuilds releases ##
```
Eg:
Release Branch: ge_current_directiof_nv
Build Version: 26449.5000.250711-1700
```
```
$BranchName = "ge_current_directiof_nv"
$BuildVersion = "26449.5000.250711-1700"
cp "\\winbuilds\release\ge_current_directiof_nv\26449.5000.250711-1700\amd64fre\vhdx\vhdx_server_serverdatacenter_en-us_vl\26449.5000.amd64fre.ge_current_directiof_nv.250711-1700_server_serverdatacenter_en-us_vl.vhdx" .
```

## Make the RiTp Scripts and Binaries Ready in Host Machine ##

```
$BuildVersion = "26449.5000.250711-1700"
$BranchName = "ge_current_directiof_nv"

$AMD_FRE_PATH = "\\winbuilds\release\$($BranchName)\$($BuildVersion)\amd64fre"
$DST_DIR = "RitpBins"

mkdir -p $DST_DIR\hcsintegration\image
mkdir -p $DST_DIR\hcsintegration\tools
mkdir -p $DST_DIR\hcsintegration\WorkingDir

cp -r $AMD_FRE_PATH\test_automation_bins\vm\containersetup\Install-ContainerHost.ps1 $DST_DIR\.
cp -r $AMD_FRE_PATH\onecoreuap_test_automation_bins\vm\hns\* $DST_DIR\.
cp -r $AMD_FRE_PATH\bin\wextest\cue\testexecution\minte\* $DST_DIR\.

# Replace master.dockerproject.org with master.dockerproject.com in Install-ContainerHost.ps1 file
(Get-Content $DST_DIR\Install-ContainerHost.ps1) -replace "master.dockerproject.org", "master.dockerproject.com" | Set-Content $DST_DIR\Install-ContainerHost.ps1

cp \\winbuilds\release\$($BranchName)\$($BuildVersion)\amd64fre\containerbaseospkgs\cbaseospkg_nanoserver_en-us\CBaseOs_$($BranchName)_$($BuildVersion)_amd64fre_NanoServer_en-us.tar.gz $DST_DIR\hcsintegration\image
cp \\redmond\1Windows\TestContent\CORE\Base\HYP\VMC\Containers\Tools\expandlayer.exe $DST_DIR\hcsintegration\tools
cp \\sesdfs.corp.microsoft.com\1windows\TestContent\CORE\Base\HYP\VMC\Containers\HcsTraceProfile.wprp $DST_DIR\hcsintegration\WorkingDir
```

## Create a New Hyper-V VM ##

#### Create VM ####
```
Hyper-V Manager
  -> New
    -> Virtual Machine
      -> Next
        -> [Virtual Machine Name - Eg: RitpTestVm]
          -> Next
            -> Generation 2
              -> Startup memory : 8000 MB
                -> Next -> Connection: [External Virtual Switch]
                  -> Next
                    -> Use an existing virtual hard disk
                      -> [Browse and Select the copied VHDX file]
                        -> Next
                          -> Finish
```

#### Increase processor count to 4 ####
```
Hyper-V Manager
  -> Right Click the Newly Created VM [Eg: RitpTestVm]
    -> Settings
      -> Processor
        -> Number of virtual processors: 4
          -> Apply
            -> Ok
```

## Enable Hyper-V Virtualisation on VM ##

#### Commands to be Executed From Host Machine ####

```
$VmName = "RitpTestVm"
Stop-VM -Name $VmName
Set-VMProcessor -VMName $VmName -ExposeVirtualizationExtensions $true
Set-VMNetworkAdapter -VMName $VmName -MacAddressSpoofing On
Start-VM -Name $VmName
```

#### Configure the OS ####
```
Username: Administrator
Password: Admin@123
```

#### Commands to be Executed From Newly Created Guest VM ####

##### Open powershell (Terminal) in Admin mode #####
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Management-PowerShell -All
```

#### Register the VM with Microsoft CorpNet ####
```
Browser: https://aka.ms/getconnected
  -> Register Device
    -> Quick Registration - Wired
      -> Employee CorpNet
        -> Select Region : APAC
          -> Select Checkbox : [Auto renew when the device connects to the network]
            -> [Mac Address of the newly created Guest VM]
              -> Description: [Hyper-V VM brought up for running Ritp Tests.]
                -> Agree & Submit
```

## Copy the newly Created Scripts and Binaries Directory (RitpBins) to Guest VM (New VM) using net use command ##

From inside the Guest VM

```
mkdir c:\data\test\bin
cd c:\data\test\bin
```
```
net use \\<ip-address|domain-name of the host vm>\c$\<Path to RitpBins directory>
```
```
Username: REDMOND\<Username>
Password: ?
```
```
cp -r \\<ip-address|domain-name of the host vm>\c$\<Path to RitpBins directory>\* .
cp -r hcsintegration c:\.
```

#### Install containerd ####
```
 .\Install-ContainerHost.ps1 -HyperV
```

## Running Ritp Tests ##

#### Finding specific Test ####
```
cmd /c 'C:\data\test\bin\te.exe .\HnsRiTp.Tests.dll /list /p:CleanHost' | sls "<Pattern prefix for test>"
```

#### Running specific test ####
```
cmd /c 'C:\data\test\bin\te.exe .\HnsRiTp.Tests.dll /Name:<Test name from prev list command> /p:CleanHost'
```

#### Run all Tests ####
```
cmd /c 'C:\data\test\bin\te.exe .\HnsRiTp.Tests.dll /Select:"@Sku=''Server'' and not @MultiMachine=true" /p:CleanHost' | tee -f c:\ritpOut.txt
```


## Important Paths ##

#### Ritp Code Path ####
```
D:\Projects\os_repo\src\onecore\vm\dv\net\hns\test\ritests\tests
```

#### Path to initiate build (bcp) command ####
```
D:\Projects\os_repo\src\onecore\vm\dv\net\hns
bcp
```

#### HNS DLL Path from os_repo ####
```
D:\Projects\os_repo\obj\amd64fre\onecore\vm\dv\net\hns\service\dll\objfre\amd64\hostnetsvc.dll
```
### Ritp Tests DLL Path ###
```
D:\Projects\os_repo\obj\amd64fre\onecore\vm\dv\net\hns\test\ritests\tests\objfre\amd64\HnsRiTp.Tests.dll
```

### Hostnetsvc DLL Path in Ritp Tests VM ###
```
c:\windows\system32\hostnetsvc.dll
```
