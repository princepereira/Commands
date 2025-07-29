## Disable firewall in HyperV VM for Host to VM connectivity with Internal NIC ##
```
netsh advfirewall set allprofiles state off
```

## Capture Firewall Events ##
```
[Inside Node]: netsh wfp capture start

[Inside Container]: Resolve-DnsName bing.com

[Inside Node]: netsh wfp capture stop
```

## Show Filters and Delete Filter ##
```
netsh wfp show filters

netsh wfp delete filter id=97494
```

## .ETL Conversion Commands ##
```
pktmon etl2txt server.etl

netsh trace convert server.etl
```

## Collect Time Travel Trace of a Windows Proces ##

Get the Process ID [Terminal 1, Terminal 2]:
```
$hnsProcessId = Get-WmiObject -Class Win32_Service -Filter "Name LIKE 'Hns'" | Select-Object -ExpandProperty ProcessId
```

Start Trace [Terminal 1]
```
.\TTTracer.exe /attach $hnsProcessId
```

Stop Trace [Terminal 2]
```
.\TTTracer.exe /stop $hnsProcessId
```
