## Kube Proxy Transition ##
```
demo/tcp-server-ipv4-cluster:tcp

Delete triggered for: 10.224.0.61
New Endpoint came up: 10.224.0.40

===>>

10.224.0.61:4444={Ready:true,Serving:true,Terminating:false,IsRemote:true}
10.224.0.78:4444={Ready:true,Serving:true,Terminating:false,IsRemote:false}

===>>

10.224.0.61:4444={Ready:false,Serving:true,Terminating:true,IsRemote:true}
10.224.0.78:4444={Ready:true,Serving:true,Terminating:false,IsRemote:false}

===>>

10.224.0.40:4444={Ready:false,Serving:false,Terminating:false,IsRemote:true}
10.224.0.61:4444={Ready:false,Serving:true,Terminating:true,IsRemote:true}
10.224.0.78:4444={Ready:true,Serving:true,Terminating:false,IsRemote:false}

===>>

10.224.0.40:4444={Ready:true,Serving:true,Terminating:false,IsRemote:true}
10.224.0.61:4444={Ready:false,Serving:true,Terminating:true,IsRemote:true}
10.224.0.78:4444={Ready:true,Serving:true,Terminating:false,IsRemote:false}

===>>

10.224.0.40:4444={Ready:true,Serving:true,Terminating:false,IsRemote:true}
10.224.0.61:4444={Ready:false,Serving:false,Terminating:true,IsRemote:true}
10.224.0.78:4444={Ready:true,Serving:true,Terminating:false,IsRemote:false}

===>>

10.224.0.40:4444={Ready:true,Serving:true,Terminating:false,IsRemote:true}
10.224.0.78:4444={Ready:true,Serving:true,Terminating:false,IsRemote:false}
```
