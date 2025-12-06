#### Get the Velocity Key Values without KirTool/Staging-Query ####
GET
```
Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides" -Name 140377743
```
DISABLE
```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides" -Name 140377743 -Value 0 -Type DWORD
Restart-Computer -force
```
ENABLE
```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides" -Name 140377743 -Value 1 -Type DWORD
Restart-Computer -force
```

#### HNS Startup Delay Debug ####

```
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name DebugStartup -PropertyType DWORD -Value 100000
Remove-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name DebugStartup
```

#### RegKey Settings for Port PreAllocation: ####
```
# Remove Port PreAllocation:
Remove-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeTCPStartPort
Remove-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeUDPStartPort
 
Remove-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeTCPPortCount
Remove-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeUDPPortCount

Restart-Service -f hns

# Verify the TCP and UDP Ports are not yet excluded
netsh int ipv4 show excludedportrange protocol=tcp store=active
netsh int ipv4 show excludedportrange protocol=udp store=active

# Preallocate the required ports by Port exclusion command
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeTCPStartPort -Value 22000
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeUDPStartPort -Value 22000
 
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeTCPPortCount -PropertyType DWORD -Value 2
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name PreallocatePortRangeUDPPortCount -PropertyType DWORD -Value 1000
 
Restart-Service -f hns
 
# Verify the required TCP and UDP Ports are preallocated
netsh int ipv4 show excludedportrange protocol=tcp store=active
netsh int ipv4 show excludedportrange protocol=udp store=active
```

#### EnableExcludedPortRange ####

**Enable/Disable the feature:**
``` New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name EnableExcludedPortRange -PropertyType DWORD -Value 1 ```
 
**Set the port range starting port:**
``` New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name ExcludedPortRangeStartPort -PropertyType DWORD -Value 50000 ```
 
**Set the port range count:**
``` New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name ExcludedPortRangePortCount -PropertyType DWORD -Value 60 ```

#### HNS Crash Dump ####
```
mkdir C:\LocalDumps
Reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\LocalDumps" /V DumpCount /t REG_DWORD /d 50 /f
Reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\LocalDumps" /V DumpType /t REG_DWORD /d 2 /f
Reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\LocalDumps" /V DumpFolder /t REG_EXPAND_SZ /d "C:\LocalDumps" /f
restart-service -f hns
```

#### DNSMaximumTTL ####
```
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name DNSMaximumTTL -Value 282
```
