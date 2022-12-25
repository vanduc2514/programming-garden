- By default, a Pod is only accessible by its internal IP address within the Kubernetes cluster
- All containers within a Pod can all reach each other's ports on localhost, and all pods in a cluster can see each other without NAT
	- Since a Pod is designed to run an instance of an application with multiple containers of the same image (known as "processes" in a Pod). Expose port of a Pod means all request come to that port will hit the load balancer (if multiple) first then it will route request to a specific container with available resource. For Pod contains only one container, request go straight to that container.
- Expose a Pod as a `Kubernetes Service` by this command
	- ```shell
	  kubectl expose deployment hello-node --type=LoadBalancer --port=8080
	  ```
- Enable `Kubernetes Service` from `minikube` by this command
	- ```shell
	  minikube service hello-node
	  ```
	- How to enable service without using `minikube` ?
	  id:: 63a8b876-bab2-4336-9e73-129f9cf5df63
		- This document [Exposing an External IP Address to Access an Application in a Cluster | Kubernetes](https://kubernetes.io/docs/tutorials/stateless-application/expose-external-ip-address/) describe the step of using `kubectl` to expose a Pod as a service
- After run this command, `minikube` dynamically allocate an available open port from a host and route it to the internal `Kubernetes Service` port at `8080`