Deploying and Running a web app using Kubernetes and docker
==================
Cluster of kubernetes includes only one coreos virtual machine.


Install virtual box and vagrant initially.

Run vagrant file by following command
~~~ sh
	"vagrant up"
~~~
With the previous command one coreos instance will be created
Now ssh to coreos instance by "vagrant ssh"
(if proxy has to be set then set https_proxy, http_proxy and no_proxy in every terminal)
Run the following commands 
	sudo mkdir -p /opt/kubernetes/bin 
	sudo chown -R core: /opt/kubernetes
	cd /opt/kubernetes
	wget https://github.com/kelseyhightower/kubernetes-coreos/releases/download/v0.0.1/kubernetes-coreos.tar.gz
	tar -C bin/ -xvf kubernetes-coreos.tar.gz
	start etcd service by "sudo systemctl start etcd"
	start docker service by "sudo <HTTP_PROXY=your_proxy> docker -d"
	open new terminal of coreos instance and start apiserver by "sudo <HTTP_PROXY=your_proxy> /opt/kubernetes/bin/apiserver --address=127.0.0.1 --port=8080 --etcd_servers=http://127.0.0.1:4001 --machines=127.0.0.1 --logtostderr=true"
	open new terminal of coreos instance and start controller-manager by "sudo <HTTP_PROXY=your_proxy> /opt/kubernetes/bin/controller-manager --master=127.0.0.1:8080 --etcd_servers=http://127.0.0.1:4001 --logtostderr=true"
	open new terminal of coreos instance and start kubelet by "sudo <HTTP_PROXY=your_proxy> /opt/kubernetes/bin/kubelet --address=127.0.0.1 --port=10250 --hostname_override=127.0.0.1 --etcd_servers=http://127.0.0.1:4001 --logtostderr=true"
	open new terminal of coreos instance and start proxy by "sudo <HTTP_PROXY=your_proxy> /opt/kubernetes/bin/proxy --etcd_servers=http://127.0.0.1:4001 -logtostderr=true"
	open new terminal of coreos instance and run kubernetes commands using kubecfg 
		to see list of pods use command "sudo <HTTP_PROXY=your_proxy> /opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 list /pods"
		to deploy and run a web app follow these commads
			create a json file specifying image details
				web.json
					{
					  "id": "web",
					  "desiredState": {
					    "manifest": {
					      "version": "v1beta1",
					      "id": "web",
					      "containers": [{
						"name": "web",
						"image": "tutum/apache-php",
						"ports": [{
						  "containerPort": 80,
						  "hostPort": 80 
						}]
					      }]
					    }
					  },
					  "labels": {
					    "name": "web"
					  }
					}
			run this command "sudo <HTTP_PROXY=your_proxy> /opt/kubernetes/bin/kubecfg -h http://127.0.0.1:8080 -c web.json create /pods" which creates a pod
			
	
	
	
	
