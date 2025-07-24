- **Build HNS** :
```
$BranchName = "official/zn_release_svc_2409b"
$NewRepo = "os_ge_cur_dir_nv"
$ProjectRoot = "D:\Projects\$NewRepo"

cd D:\Projects

favbranch /BranchName:official/$BranchName

gvfs clone https://microsoft@dev.azure.com/microsoft/OS/_git/os.2020 -b $BranchName $NewRepo

cd $ProjectRoot\src\utilities

.\razzle.ps1

cd $ProjectRoot\src\onecore\base\wil\containment

bcp

cd $ProjectRoot\src\onecore\base\wil\staging

bcp

cd $ProjectRoot\src\onecore\services\

bcp
```
```
cd $ProjectRoot\src\onecore\vm\dv\net\hns

bcp
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

