- Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods.
- Multiple Pods can reference the same Secret.
- Secrets are similar to [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) but are specifically intended to hold confidential data.
- **CAUTION**
	- Kubernetes Secrets are, by default, stored unencrypted in the API server's underlying data store (etcd)
	- Anyone with API access can retrieve or modify a Secret, and so can anyone with access to etcd. Additionally, anyone who is authorized to create a Pod in a namespace can use that access to read any Secret in that namespace
- **SECRET TYPES**
	- `Opaque` -> default; arbitrary user-defined data as **key-value** pairs
	- `kubernetes.io/service-account-token` -> a token that identifies the service account to the Kubernetes API, use for authenticate with other Kubernetes Cluster
	- `kubernetes.io/dockercfg` -> a serialized `~/.dockercfg` file for authorize with Docker Registry
	- `kubernetes.io/dockerconfigjson` -> a serialized `~/.docker/config.json` file for authorize with Docker Registry
	- `kubernetes.io/basic-auth` -> Basic Secret with `username` and `password`
	- `kubernetes.io/ssh-auth` -> private SSH key
	- `kubernetes.io/tls` -> TLS Secret type
	- TODO `bootstrap.kubernetes.io/token` -> Stored tokens used to sign well known `ConfigMaps`
- Use Secret for a Pod
	- As [container environment variable](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables).
		- Fastest and easiest way of using secret for a Pod -> Secrets will be applied for all containers inside that Pod
	- As [files](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-files-from-a-pod) in a [volume](https://kubernetes.io/docs/concepts/storage/volumes/) mounted on one or more of its containers.
		- Complex way of using secret, have to create a volume and mount it to the Pod for all containers
	- By the [kubelet when pulling images](https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets) for the Pod.
		- TODO Not yet investigated!
- Creating a Secret Using Configuration (Manifest) file #recommended
	- Create manifest `.yaml`
		- Using `data`, all value must be encoded by `base64`
		- ```yaml
		  apiVersion: v1
		  kind: Secret
		  metadata:
		    name: mysecret
		  type: Opaque
		  data:
		    username: YWRtaW4=
		    password: MWYyZDFlMmU2N2Rm
		  ```
		- Using `stringData`, value can be stored as raw string value, then it will be converted to `base64` when created.
		- ```yaml
		  apiVersion: v1
		  kind: Secret
		  metadata:
		    name: mysecret
		  type: Opaque
		  stringData:
		    username: admin
		    password: 1f2d1e2e67df
		  ```
	- Apply secret to Kubernetes Namespace using
		- ```shell
		  kubectl apply -f ./secret.yaml
		  ```
	- Secret can also be edited from the manifiest file `.yaml` and can be re-applied for a Namespace
-