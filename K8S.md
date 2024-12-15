# Kubernetes Cheat Sheet

## Commands

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
