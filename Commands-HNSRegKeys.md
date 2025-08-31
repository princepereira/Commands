#### HNS Startup Delay Debug ####

```
New-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name DebugStartup -PropertyType DWORD -Value 100000
Remove-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Services\hns\State -Name DebugStartup
```

