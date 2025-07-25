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

## .ETL Conversion Commands ##
```
pktmon etl2txt server.etl

netsh trace convert server.etl
```
