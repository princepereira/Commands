## Kube Proxy Endpoint Transition ##
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

## Build KubeProxy in Windows ##

Steps are taken from: 
- [preparing-your-local-operating-system](https://github.com/kubernetes/community/blob/master/contributors/devel/development.md#preparing-your-local-operating-system)
- [building-kubernetes-for-windows-from-source](https://github.com/kubernetes/community/blob/master/sig-windows/CONTRIBUTING.md#building-kubernetes-for-windows-from-source)

1. Install WSL: https://docs.microsoft.com/en-us/windowwsls/wsl/install
	1. Make Sure you use WSL 2 backend. This is the default today (03/11/2022).
I am using Ubuntu 20.04. This is also the default today.

2. Open WSL. Rest of the commands should be executed in WSL.
3. Update WSL: `sudo apt update -y && sudo apt upgrade -y`
4. Install GO inside WSL:
```
	https://go.dev/doc/install
	rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.8.linux-amd64.tar.gz
```
Run `go version` to verify go is installed
<br/>
5. Install Docker Desktop for WSL. Ensure it works.
<br/>
6. Install packages: `sudo apt install jq ; 	sudo apt install rsync ; `
<br/>
7. Add following to /etc/profile:
```
	export PATH=$PATH:/usr/local/go/bin
	export working_dir="$(go env GOPATH)/src/k8s.io"
	export user="<my_github_username>"
	
	#========Final=========#
	export GOROOT=/usr/local/go
	export GOPATH=/root/GoProjects
	export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
	export GO111MODULE=on
	export working_dir="$(go env GOPATH)/src/k8s.io"
	export user="princepereira"
	export GITHUB_USER=princepereira
```
8. Refresh env using: `source /etc/profile `
9. Fork Kubernetes repo in GitHub and run the following:
```
	mkdir -p $working_dir
	cd $working_dir
	git clone https://github.com/$user/kubernetes.git
```
10. Add & configure remote:
```
cd $working_dir/kubernetes
git remote add upstream https://github.com/kubernetes/kubernetes.git
# Never push to upstream master
	git remote set-url --push upstream no_push
# Confirm that your remotes make sense:
	git remote -v
```
This completes the one-time setup. The following steps will be done repeatedly as you iterate on new features/fixes.

11. Rebase and setup feature branch:
```
cd $working_dir/kubernetes
git fetch upstream
git checkout master
git rebase upstream/master
git checkout -b "my-branch-for-feature"
```

12. To build kube-proxy inside build container: `	./build/run.sh make kube-proxy KUBE_BUILD_PLATFORMS=windows/amd64 `
	

NOTE: The build can take a long time to run the first time as it creates a large build container. For me the first time building took ~45 minutes.

Once completed you should see this:

 pro tip: if you add GOLDFLAGS="" GOGCFALGS="all=-N -l" to the end you can attach a debugger. NOTE: I have not verified this.

13. You should see the compiled kube-proxy.exe here: `cd _output/dockerized/bin/windows/amd64 `

14. Copy it to Windows using: ` cp kube-proxy.exe /mnt/c/users/$user/Documents `


## 16. How to cherry pick ##
- WSL Ubuntu
- Go to Kubernetes Projects
- Execute CherryPick script
- Command: ` hack/cherry_pick_pull.sh upstream/<K8S Target Release Branch> <Master PR Number> `
  <br/> Eg: ` hack/cherry_pick_pull.sh upstream/release-3.14 98765 `
- Update release note, and /kind


