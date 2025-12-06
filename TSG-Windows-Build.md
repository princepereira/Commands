- **Build HNS** :
```
$BranchName = "official/zn_release_svc_2409b"
$NewRepo = "os_ge_cur_dir_nv"
$ProjectRoot = "D:\Projects\$NewRepo"

cd D:\Projects

favbranch /BranchName:$BranchName

gvfs clone https://microsoft@dev.azure.com/microsoft/OS/_git/os.2020 -b $BranchName $NewRepo
```

Build No Opt Chk Build (Debug Build)

```
cd $ProjectRoot\src

cmd

.\tools\razzle.cmd amd64chk no_opt
```

For Free Build

```
cd $ProjectRoot\src\utilities

.\razzle.ps1
```

```
# enable code signing in razzle
SET NT_SIGNCODE=1

cd $ProjectRoot\src\onecore\base\wil\containment

bcp

cd $ProjectRoot\src\onecore\base\wil\staging

bcp

cd $ProjectRoot\src\onecore\services\

bcp

cd $ProjectRoot\src\onecore\vm\dv\net\hns

bcp

HNS DLL Location : $ProjectRoot\obj\amd64fre\onecore\vm\dv\net\hns\service\dll\objfre\amd64\HostNetSvc.dll
```
- **Vpack refresh issue during razzle run** : `Restart the computer`

- **Did not find required feature: Feature_SFS_CD_BugFixes_2509 in FeatureStaging-StorageAndFileSystems.xml** :
```
$ProjectRoot = "D:\Projects\os_ge_cur_dir_nv"
```
```
rm -r -Force $ProjectRoot\obj\*
```
```
cd $ProjectRoot\src\onecore\base\wil\containment
```
```
bcp
```
```
cd $ProjectRoot\src\onecore\base\wil\staging
```
```
bcp
```
```
cd $ProjectRoot\src\onecore\services\
```
```
bcp
```
```
cd $ProjectRoot\src\onecore\vm\dv\net\hns
```
```
bcp
```

- **Add Trace Logging in HNS**

```

#include "HnsLogging.h"

TraceLoggingWrite(g_HnsLoggingProvider,
                    "endPtPort->GetPortSettings() failed....",
                    TraceLoggingLevel(TRACE_LEVEL_ERROR),
                    TraceLoggingBoolean(endpoint->IsAttached(), "IsAttached"));

TraceLoggingWrite(g_HnsLoggingProvider,
            "Prince ========== ",
            TraceLoggingLevel(TRACE_LEVEL_ERROR),
            TraceLoggingValue("Some data", "Description"));
		


TraceLoggingWrite(g_HnsLoggingProvider,
            "Prince ==== UpdatePolicy DisableModifyWinFwRules started ",
            TraceLoggingLevel(TRACE_LEVEL_VERBOSE));


TraceLoggingWrite(g_HnsLoggingProvider,
            "Prince ========== started calling instance->Modify inside Modify of EntityManager.h",
            TraceLoggingLevel(TRACE_LEVEL_VERBOSE),
            TraceLoggingValue("Some data", "Description"));
```

- **Binaries Path inside Node**

```
vfpext.sys ---> C:\Windows\system32\drivers\vfpext.sys

vfpapi.dll ---> C:\Windows\system32\vfpapi.dll

vfpctrl.exe ---> C:\Windows\system32\vfpctrl.exe

hostnetsvc.dll ---> C:\Windows\system32\hostnetsvc.dll

kube-proxy.exe ---> C:\k\kube-proxy.exe

Tcpip.sys ---> C:\Windows\system32\drivers\tcpip.sys

Netio.sys ---> C:\Windows\system32\drivers\netio.sys
```

- **Binaries Path in Source Code**
```
HNS: os_fe_22_2306b\obj\amd64fre\onecore\vm\dv\net\hns\service\dll\objfre\amd64\HostNetSvc.dll

TCPIP.SYS: D:\Projects\os_fe_22_2306b\obj\amd64fre\minio\netio\driver\sys\nt\objfre\amd64\tcpip.sys

NETIO.SYS: D:\Projects\os_fe_22_2306b\obj\amd64fre\minio\netio\sys\nt\objfre\amd64\netio.sys

VfpBinary Path : D:\Projects\os_fe_22\obj\amd64fre\onecore\services\vfp\driver\ext\objfre\amd64

VfpCtrlPath : D:\Projects\os_fe_22\obj\amd64fre\onecore\services\vfp\control\vfpctrl\objfre\amd64

VfpApi : D:\Projects\os_fe_22_2311b\obj\amd64fre\onecore\services\vfp\control\vfpapi\dll\objfre\amd64\vfpapi.dll
```

- **Enable test signing in the VM**
```
    bcdedit /set TESTSIGNING ON
    bcdedit /debug on
    bcdedit /bootdebug on

As per Sourav inside VM===

bcdedit /set TESTSIGNING ON
bcdedit /debug off
bcdedit /bootdebug off<img width="912" height="225" alt="image" src="https://github.com/user-attachments/assets/3c963b85-ccef-4f05-afbf-d44505b87524" />
```

- **Compile and build**
```
Bcp
Bcz
Bz  // Build parents
```
