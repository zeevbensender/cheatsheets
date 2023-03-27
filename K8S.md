# Kubernetes Cheat Sheet

## Commands

* ### Compare deployed yaml file changes with deployed resources
  ```
  $ kubectl diff -f <file name>.yaml
  ```
* ### Show resources list
   ```
   $ kubectl api-resources
   ```
* ### Show resource properties
  ```
  $ kubectl explain <resource name> [--recursive]
  ```
