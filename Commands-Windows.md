## Netsh Commands to capture HNS Traces ##
```
netsh trace start provider="{0C885E0D-6EB6-476C-A048-2457EED3A5C1}" level=6 tracefile=server.etl report=di persistent=yes
 
netsh trace stop
 
netsh trace convert server.etl
```

## Pktmon Commands to capture HNS Traces ##
```
pktmon start --trace -p Microsoft-Windows-Host-Network-Service -l 6 -f traces.etl

pktmon stop

ptmon etl2txt traces.etl
```

Multifile Capture
```
pktmon start --trace -p Microsoft-Windows-Host-Network-Service -l 2 -m multi-file

pktmon stop

ptmon etl2txt traces.etl
```

## Pktmon Commands to capture HNS & VFP Traces ##
```
pktmon start --trace -p Microsoft-Windows-Host-Network-Service -p Microsoft-Windows-Hyper-V-VfpExt -l 6 -f traces.etl

pktmon stop

ptmon etl2txt traces.etl
```

## Pktmon Commands to capture HNS & VFP Traces along with Packet Capture ##
```
pktmon start --trace -c -p Microsoft-Windows-Host-Network-Service -p Microsoft-Windows-Hyper-V-VfpExt -l 6 -f traces.etl

pktmon stop

ptmon etl2txt traces.etl
```

## TCP Traces ##
```
	Start Trace: netsh trace start provider=Microsoft-Windows-TCPIP level=5 tracefile=tcpip.etl report=disabled perf=no 
	Trace Path: tcpip.etl
	Stop Trace: netsh trace stop
	Convert to Text: netsh trace convert tcpip.etl
	Search for (router Advertisement): router
	
	More Advanced (To find the caller of nsi api):
	
	How to check who is deleting the route:
	 
		• Start Capture: netsh trace start provider=Microsoft-Windows-TCPIP level=5 provider="{EB004A05-9B1A-11D4-9123-0050047759BC}" keywords=0xFFFFFFFFFFF level=5 tracefile=tcpip.etl report=disabled perf=no fileMode=circular maxsize=512
		• Repro
		• Stop Capture : netsh trace stop
		• Convert Capture : netsh trace convert tcpip.etl tmfpath=\\winbuilds\release\fe_release_svc_2308b\20348.30000.230620-1700\amd64fre\symbols.pri\TraceFormat
		• Look at the converted logs where Log Format with Process ID: 
[0]08FC.2CB4::2023/06/22-18:55:06.496848300 [nt]NSI: Enter>>NsiSetAllParametersEx. ModuleId:{eb004a01-9b1a-11d4-9123-0050047759bc} ObjectIndex:16 Store:1 ProcessId:2300 CompartmentId:1 CompartmentScope:0.
[0]08FC.2CB4::2023/06/22-18:55:06.496885100 [Microsoft-Windows-TCPIP]IP: Route change notification. DestinationPrefix = fd41:e387:fae0:f434::5/128, NextHop = fe80::1234:5678:9abc, Interface = 9, Compartment = 1, NotifyFlags = 0x2.
[0]08FC.2CB4::2023/06/22-18:55:06.496886500 [Microsoft-Windows-TCPIP]IP: Route 0xFFFFE50CCCA90770 deleted on interface 9, Protocol = IPv6, DestinationPrefix = 0.0.0.0 6((Ignore IPv4 address), IPv6 address =) fd41:e387:fae0:f434::5 /128, Nexthop = 0.0.0.0 6((Ignore IPv4 address), IPv6 address =) fe80::1234:5678:9abc.
		• Look for Process ID: tasklist /svc | sls "2300"
     svchost.exe                   2300 hns
```

## Pktmon Capture with HNS, TCPIP, VFP, NDIS and packet Capture on All Compartments ##
```
pktmon start --capture --comp all --trace -p Microsoft-Windows-TCPIP -k 0xFF -l 6 -p Microsoft-Windows-NDIS -l 6 -p Microsoft-Windows-Host-Network-Service -l 6 -p Microsoft-Windows-Hyper-V-VfpExt -l 6 -f traces.etl --pkt-size 100 -s 1024
```

## Cilium Stack Captures##
```
rm -Force server.etl ; rm -Force server.txt ; netsh trace start provider="{394f321c-5cf4-404c-aa34-4df1428a7f9c}" level=6 provider="{f2f2ca01-ad02-4a07-9e90-95a2334f3692}" level=5 provider="{00e7ee66-5b24-5c41-22cb-af98f63e2f90}" level=5 provider="{425f4e76-8d2d-506f-57d6-ab466512e14b}" level=5 provider="{6ff0db31-e888-5328-7cb4-33b76702e6e8}" level=5  provider="{0C885E0D-6EB6-476C-A048-2457EED3A5C1}" level=5 provider="{580bbdea-b364-4369-b291-d3539e35d20b}" level=5 tracefile=server.etl report=di persistent=yes


netsh trace stop ; netsh trace convert server.etl
```

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

## Time Travel Trace ##
```
.reload wcnagent.dll

bm wcnagent!*WcnAgentDeleteLoadBalancer

bm wcnagent!wcna::update_loadbalancer



○ \\dbg\privates\LKG\uncompressed\amd64\TTD  

- Get the Hns Process ID  (in Terminal 1, 2)
○ $hnsProcessId = Get-WmiObject -Class Win32_Service -Filter "Name LIKE 'Hns'" | Select-Object -ExpandProperty ProcessId

- Start tttracer with Hns Process ID (in Terminal 1)
○ .\TTTracer.exe /attach $hnsProcessId

- Reproduce the issue while tttracer is running

- Stop the tttracer once the issue is reproduced  (in Terminal 2)
.\TTTracer.exe /stop $hnsProcessId
```
