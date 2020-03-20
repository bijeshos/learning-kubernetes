# kubectl cheat sheet


## Creating Objects

- `kusbectl create -f ./my-manifest.yaml`          # create resource(s)
- `kubectl create -f ./my1.yaml -f ./my2.yaml`     # create from multiple files
- `kubectl create -f ./dir`                        # create resource(s) in all manifest files in dir
- `kubectl create -f https://git.io/vPieo`         # create resource(s) from url
- `kubectl run nginx --image=nginx`                # start a single instance of nginx
- `kubectl explain pods,svc  `                     # get the documentation for pod and svc manifests

  
## Viewing, Finding Resources

### Get commands with basic output
- `kubectl get services `                         # List all services in the namespace
- `kubectl get pods --all-namespaces    `         # List all pods in all namespaces
- `kubectl get pods -o wide `                     # List all pods in the namespace, with more details
- `kubectl get deployment my-dep `                # List a particular deployment
- `kubectl get pods --include-uninitialized   `   # List all pods in the namespace, including - uninitialized ones

### Describe commands with verbose output
- `kubectl describe nodes my-node`
- `kubectl describe pods my-pod`
- `kubectl get services --sort-by=.metadata.name` # List Services Sorted by Name

### List pods Sorted by Restart Count
- `kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'`

### Get the version label of all pods with label app=my-app
- `kubectl get pods --selector=app=my-app rc -o \
  jsonpath='{.items[*].metadata.labels.version}'`

### Get all running pods in the namespace
- `kubectl get pods --field-selector=status.phase=Running`

### Get ExternalIPs of all nodes
- `kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'`


### List Events sorted by timestamp
- `kubectl get events --sort-by=.metadata.creationTimestamp`

## Updating Resources

- `kubectl set image deployment/frontend www=image:v2   `            # Rolling update "www" containers of "frontend" deployment, updating the image
- `kubectl rollout undo deployment/frontend     `                    # Rollback to the previous deployment
- `kubectl rollout status -w deployment/frontend   `                 # Watch rolling update status of "frontend" deployment until completion

### Force replace, delete and then re-create the resource. Will cause a service outage.
- `kubectl replace --force -f ./pod.json`

## Scaling Resources

- `kubectl scale --replicas=3 rs/foo  `                               # Scale a replicaset named 'foo' to 3
- `kubectl scale --replicas=3 -f foo.yaml  `                          # Scale a resource specified in "foo.yaml" to 3
- `kubectl scale --current-replicas=2 --replicas=3 deployment/mysql`  # If the deployment named mysql's current size is 2, scale mysql to 3
- `kubectl scale --replicas=5 rc/foo rc/bar rc/baz        `           # Scale multiple replication controllers

## Deleting Resources

- `kubectl delete -f ./pod.json `                                             # Delete a pod using the type and name specified in pod.json
- `kubectl delete pod,service baz foo  `                                      # Delete pods and services with same names "baz" and "foo"
- `kubectl delete pods,services -l name=myLabel  `                            # Delete pods and services with label name=myLabel
- `kubectl delete pods,services -l name=myLabel --include-uninitialized  `    # Delete pods and services, including uninitialized ones, with label name=myLabel
- `kubectl -n my-ns delete po,svc --all        `                              # Delete all pods and services, including uninitialized ones, in namespace my-ns,

## Interacting with running Pods

- `kubectl logs my-pod     `                            # dump pod logs (stdout)
- `kubectl logs my-pod --previous `                     # dump pod logs (stdout) for a previous instantiation of a container
- `kubectl logs my-pod -c my-container  `               # dump pod container logs (stdout, multi-container case)
- `kubectl logs my-pod -c my-container --previous `     # dump pod container logs (stdout, multi-container case) for a previous instantiation of a container
- `kubectl logs -f my-pod      `                        # stream pod logs (stdout)
- `kubectl logs -f my-pod -c my-container `             # stream pod container logs (stdout, multi-container case)
- `kubectl run -i --tty busybox --image=busybox -- sh`  # Run pod as interactive shell
- `kubectl attach my-pod -i   `                         # Attach to Running Container
- `kubectl port-forward my-pod 5000:6000 `              # Listen on port 5000 on the local machine and forward to port 6000 on my-pod
- `kubectl exec my-pod -- ls /      `                   # Run command in existing pod (1 container case)
- `kubectl exec my-pod -c my-container -- ls /   `      # Run command in existing pod (multi-container case)
- `kubectl top pod POD_NAME --containers `              # Show metrics for a given pod and its containers

## Resource types

- `kubectl api-resources`
- `kubectl api-resources --namespaced=true `     # All namespaced resources
- `kubectl api-resources --namespaced=false `    # All non-namespaced resources
- `kubectl api-resources -o name         `       # All resources with simple output (just the resource name)
- `kubectl api-resources -o wide    `            # All resources with expanded (aka "wide") output
- `kubectl api-resources --verbs=list,get  `     # All resources that support the "list" and "get" request verbs
- `kubectl api-resources --api-group=extensions` # All resources in the "extensions" API group



