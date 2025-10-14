## Trace Collection From The Node ##
```
cd C:\wcn\scripts

.\trace.ps1 -operation "Start" -verboseTracing

# ... reproduce the issue ...

.\trace.ps1 -operation "Stop"

.\trace.ps1 -operation "Collect"

Logs/Traces available at : C:\wcn\debug\archive\
```

## Start E2ETest ##
```
git clone https://mscodehub.visualstudio.com/NGServerContainerNetworking/_git/e2e-test
```

Set [`InstallRequired`](https://mscodehub.visualstudio.com/NGServerContainerNetworking/_git/e2e-test?path=/aks-test/testconf.json&version=GBmain&line=9&lineEnd=9&lineStartColumn=1&lineEndColumn=34&lineStyle=plain&_a=contents) boolean in `e2e-test\aks-test\testconf.json` to `true`

```
.\starttest.ps1 -testsResults ([ref]$false) -BinaryDirectory "$PWD\bin" -NodepoolName npwin
```
