#### #Docker
- **Purpose:** Package and distribute applications with all their dependencies into containers.
- **Main Functionality:** Creates lightweight, consistent environments (containers) that can run applications across different stages of development.
- **Use Case:** Developers use Docker to ensure their applications run consistently in various environments (development, testing, production).

#### #Kubernetes (K8s):

- **Purpose:** Orchestrate and manage the deployment, scaling, and operation of containerized applications.
- **Main Functionality:** Coordinates the deployment of multiple containers in a cluster, providing features like automated scaling, load balancing, and self-healing.
- **Use Case:** Operations teams use Kubernetes to efficiently manage and scale containerized applications in a production environment.

**Relationship:**

- Docker is primarily used to package and create containers.
- Kubernetes is used to orchestrate and manage the deployment of those containers at scale.
  
  ```shell
cat <<EOF >static-web.yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-web
  labels:
    role: myrole
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
EOF

```shell
    KUBELET_ARGS="--cluster-dns=10.254.0.10 --cluster-domain=kube.local --manifest-url=static-web.yaml"
    ```
```


