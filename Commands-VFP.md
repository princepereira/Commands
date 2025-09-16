#### Different Type Ports: ####

```
External Port:
	- Port Friendly name    : ExternalPort
Host Port:
	- Port Friendly name    : Container NIC 8453379c
Pod port:
	- Port Friendly name    : B5F4E943-756D-4EE8-A70C-DF5AD7C74715
```

#### How to list all Ports: #####
``` vfpctrl /list-vmswitch-port ```

#### How to list Host Port: ####
``` vfpctrl /list-vmswitch-port | Select-String -Pattern "Container" -Context 5,5 ```

#### Get the Port Name and find the layers: ####
``` vfpctrl /port 1666B12E-5204-46FB-A42E-1295AE2DF63A /list-layer ```

#### List Group for the Layer: ####
``` vfpctrl.exe /port F2A203DC-F7AB-4354-B969-8F60C466685A /layer LB_DSR /list-group ```

#### List Rule for the Group: ####
``` vfpctrl.exe /port F2A203DC-F7AB-4354-B969-8F60C466685A /layer LB_DSR /group LB_DSR_IPv4_IN /list-rule ```

#### List unified rules for the Port: ####
``` vfpctrl.exe /port 1666B12E-5204-46FB-A42E-1295AE2DF63A /list-unified-flow
vfpctrl.exe /list-vmswitch-port | select-string -Pattern "Container" -Context 5,5
 
  Port name             : 4FD72AB0-2C35-4AE7-AC9A-F893495BD5EF
> Port Friendly name    : Container NIC 6c50381c
  Switch name           : 04F2CC76-AE71-4E04-9B0B-D3F27D8BC4E3
  Switch Friendly name  : ext
  PortId                : 1
  VMQ Usage             : 0
  SR-IOV Usage          : 0
 
 
$port="4FD72AB0-2C35-4AE7-AC9A-F893495BD5EF"
 
PS C:\Users\azureuser> vfpctrl.exe /list-flow /layer SLB_LB_LAYER /port $port
```

#### VFP get port settings ####

```
vfpctrl.exe /port $cPort /get-port-flow-settings
```

#### VFP set port settings ####
```
vfpctrl.exe /port $portGuid /set-port-flow-settings "240 15 240 240 1400 1000 2000 1 1 0 1 1 1 0 0 0 100000 0 0 0 0 1 50 1 0 0 0 10000 1"
```

#### Find the Port ID ####

```
$clientPodIP= "10.244.2.125" ; $mac = (Get-HnsEndpoint | Where IPAddress -Eq $clientPodIP).MacAddress

$data = vfpctrl.exe /list-vmswitch-port | sls $mac -Context 20,1

$port = $data -split "`n" | Where-Object { $_ -match "^\s*Port name\s*:" } | ForEach-Object {
    ($_ -split ":", 2)[1].Trim()
}
```

#### Find the LB_DSR Rule ####

```
$curNodeIp = "10.224.0.6" ; $vip= "10.0.199.211" ; $intPort = "4444" ; $extPort = "4444"

vfpctrl /port $port /layer LB_DSR /group LB_DSR_IPv4_OUT /list-rule | sls "LB_DSR_$curNodeIp`_$vip`_$intPort`_$extPort" -Context 1,46
```	

#### VFP Add Layer ####

```
$p = <Port Name from the command: vfpctrl /list-vmswitch-port>

vfpctrl.exe /port $p /add-layer "INTERNAL_L2_REWRITE_LAYER INTERNAL_L2_REWRITE_LAYER stateless 75 0"
```

#### VFP Add Group ####

```
$p = <Port Name from the command: vfpctrl /list-vmswitch-port>

vfpctrl.exe /port $p /layer INTERNAL_L2_REWRITE_LAYER /add-group "INTERNAL_L2_REWRITE_GROUP_IPV4_OUT INTERNAL_L2_REWRITE_GROUP_IPV4_OUT out 0  lpm VfxConditionDestinationIp"
```

#### VFP Add Rule ####

```
$p = <Port Name from the command: vfpctrl /list-vmswitch-port>

vfpctrl /port $p /layer LB_DSR /group LB_DSR_IPv4_OUT /add-rule-ex "lbnat lbnat * * * * * 3 240 500 lbnat 0 0/0 720896 169.254.95.145 4444"
```
