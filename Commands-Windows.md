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

## Collect Time Travel Trace of a Windows Process ##

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

## Capture packets with VLAN tagging using Pktmon  ##
```
pktmon start --capture --comp all --flags 0x0ff -m multi-file

pktmon stop

pktmon etl2txt <server.etl>
```

## Kir Tool / Staging Tool ##

[KirTool](https://www.osgwiki.com/wiki/WSD_Wiki-KIRTool)

Execute only one time: `.\kirtool.exe telemetry-optin`

```
.\kirtool.exe staging query <bugid>
.\kirtool.exe staging enable <bugid>
.\kirtool.exe staging disable <bugid>
```

## Net use for copying ##
```
net use \\172.29.48.1\c$\Shared

net use \\server-d22\shared /user:redmond\ppereira /savecred

net use \\ppereira-idc\Users\ppereira\Yamls\AksTestAutomation /user:redmond\ppereira
```

## Robo Copy ##
```
robocopy "C:\SourceFolder" "D:\BackupFolder" /E

robocopy "C:\SourceFolder" "\\172.29.48.1\c$\Shared\BackupFolder" /E
```
