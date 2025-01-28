# Kubernetes Cheat Sheet
## kubectl
Set the Node Configuration
```bash
export KUBECONFIG=PATH_TO_CONFIG_FILE/config
```
## Commands
* ### Diagnostics
  #### Describe resource in json format
  ```bash
  kubectl get pvc MY_PVC -n NAMESPACE -o json
  ```
  ### Get storage class name of <b>pvc</b> with <b>jsonpath</b>
  ```bash
  kubectl get pvc MY_PVC -n NAMESPACE -o jsonpath='{.spec.storageClassName}' ; echo
  ```


* ### Run test pod for various validations
  ```bash
  kubectl run test-pod --rm -it --image=busybox --restart=Never -- sh
  ```

* ### Compare deployed yaml file changes with deployed resources
  ```bash
  kubectl diff -f <file name>.yaml
  ```
* ### Show resources list
   ```bash
  kubectl api-resources
   ```
* ### Show resource properties
  ```bash
  kubectl explain <resource name> [--recursive]
  ```
* ### Show external IPs
```bash
kubectl get svc -A | awk ' { print $5 }' | grep -v none
```

## Tricks
### Find an entity managing another k8s entity
```bash
kubectl get <resource-type> <resource-name> -n <namespace> -o yaml | less
```
Find <b>ownerReferences</b> in the output
