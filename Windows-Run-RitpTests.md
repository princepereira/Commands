## Copy a VHDX image from winbuilds releases ##
```
Eg:
Release Branch: ge_current_directiof_nv
Build Number: 26449.5000.250711-1700
```
```
$BranchName = "ge_current_directiof_nv"
$BuildNumber = "26449.5000.250711-1700"
cp "\\winbuilds\release\ge_current_directiof_nv\26449.5000.250711-1700\amd64fre\vhdx\vhdx_server_serverdatacenter_en-us_vl\26449.5000.amd64fre.ge_current_directiof_nv.250711-1700_server_serverdatacenter_en-us_vl.vhdx" .
```

## Create a New Hyper-V VM ##

### Create VM ###
```
Hyper-V Manager
  -> New
    -> Virtual Machine
      -> Next
        -> [Virtual Machine Name - Eg: RitpTestsVm]
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

### Increase processor count to 4 ###
```
Hyper-V Manager
  -> Right Click the Newly Created VM [Eg: RitpTestsVm]
    -> Settings
      -> Processor
        -> Number of virtual processors: 4
          -> Apply
            -> Ok
```

## Enable Hyper-V Virtualisation on VM ##

### Commands to be Executed From Host Machine ###

```
$VmName = "RitpTestsVm"
Stop-VM -Name $VmName
Set-VMProcessor -VMName $VmName -ExposeVirtualizationExtensions $true
Set-VMNetworkAdapter -VMName $VmName -MacAddressSpoofing On
Start-VM -Name $VmName
```

### Commands to be Executed From Newly Created Guest VM ###

#### Open powershell (Terminal) in Admin mode ####
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Management-PowerShell -All
```


## Make the RiTp Scripts and Binaries Ready ##

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

cp \\winbuilds\release\$($BranchName)\$($BuildVersion)\amd64fre\containerbaseospkgs\cbaseospkg_nanoserver_en-us\CBaseOs_$($BranchName)_$($BuildVersion)_amd64fre_NanoServer_en-us.tar.gz $DST_DIR\hcsintegration\image
cp \\redmond\1Windows\TestContent\CORE\Base\HYP\VMC\Containers\Tools\expandlayer.exe $DST_DIR\hcsintegration\tools
cp \\sesdfs.corp.microsoft.com\1windows\TestContent\CORE\Base\HYP\VMC\Containers\HcsTraceProfile.wprp $DST_DIR\hcsintegration\WorkingDir
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

### Install containerd ###
```
 .\Install-ContainerHost.ps1 -HyperV

```

## Running Ritp Tests ##

### Finding specific Test ###
```
cmd /c 'C:\data\test\bin\te.exe .\HnsRiTp.Tests.dll /list /p:CleanHost' | sls "<Pattern prefix for test>"
```

### Running specific test ###
```
cmd /c 'C:\data\test\bin\te.exe .\HnsRiTp.Tests.dll /Name:<Test name from prev list command> /p:CleanHost'
```

### Run all Tests ###
```
cmd /c 'C:\data\test\bin\te.exe .\HnsRiTp.Tests.dll /Select:"@Sku=''Server'' and not @MultiMachine=true" /p:CleanHost' | tee -f c:\ritpOut.txt
```

