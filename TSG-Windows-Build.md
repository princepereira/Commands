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

