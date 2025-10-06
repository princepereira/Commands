## Trace Collection From The Node ##
```
cd C:\wcn\scripts

.\trace.ps1 -operation "Start" -verboseTracing

# ... reproduce the issue ...

.\trace.ps1 -operation "Stop"

.\trace.ps1 -operation "Collect"

Logs/Traces available at : C:\wcn\debug\archive\
```
