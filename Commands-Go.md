## Run a single test ##

Always run on Windows Node on this directory Path: C:\Users\ppereira\GoProjects\k8s.io\kubernetes>

```
- go test -timeout 30s -run ^TestCreateServiceVip$ k8s.io/kubernetes/pkg/proxy/winkernel
- go test -timeout 30s -run ^TestGetAllEndpointsByNetworkWithDupEP$ k8s.io/kubernetes/pkg/proxy/winkernel
- go test -timeout 30s -run ^TestUpdateLoadBalancerInNonEbpfMode$ k8s.io/kubernetes/pkg/proxy/winkernel
- go test -timeout 30s -run ^TestClusterIPLBWithL4ProxyAnnotation$ .\pkg\proxy\winkernel\
```


## Run File Test ##

```
go test -timeout 30s -run "^(TestCreateServiceVip|TestCreateRemoteEndpointOverlay|TestCreateRemoteEndpointL2Bridge|TestSharedRemoteEndpointDelete|TestSharedRemoteEndpointUpdate|TestCreateLoadBalancer|TestCreateDsrLoadBalancer|TestClusterIPLBInCreateDsrLoadBalancer|TestEndpointSlice|TestNoopEndpointSlice|TestFindRemoteSubnetProviderAddress)$" k8s.io/kubernetes/pkg/proxy/winkernel
```

## Run all tests with coverage ##

In Linux/WSL
```
go test -v -cover .\\...
```

## Run all tests without coverage ##
```
go test -v ./... -count=1
```
