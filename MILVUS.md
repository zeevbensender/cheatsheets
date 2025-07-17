# MILVUS

## Troubleshooting

### Exploring minio buckets

Deploy the temporary pod with the ```mc``` utility installed in the container:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mc-client
  namespace: <namespace>
spec:
  containers:
    - name: mc
      image: minio/mc
      command: [ "sleep", "3600" ]  # Keep the pod running
      imagePullPolicy: IfNotPresent
  restartPolicy: Never
```

Apply the yaml and exec into it:
```bash
kubectl apply -f mc-pod.yaml
kubectl exec -it mc-client -n <namespace> -- sh
```

Configure the mc:
```bash
mc alias set local http://<minio url>:9000 <access_key> <secret_key>
```

Explore the buckets:
```bash
mc ls local
```

## Python Scripts

### Load Collection
```python
import sys
from pymilvus import connections, Collection, exceptions

def load_collection(host_name, collection_name, port="19530", alias="default"):
    try:
        print(f"Connecting to Milvus at {host_name}:{port}...")
        connections.connect(alias=alias, host=host_name, port=port)

        print(f"Attempting to load collection '{collection_name}'...")
        collection = Collection(collection_name, using=alias)

        collection.load()
        collection.wait_for_loading()

        print(f"✅ Collection '{collection_name}' successfully loaded and ready.")
    except exceptions.MilvusException as e:
        print(f"❌ Failed to load collection '{collection_name}': {e}")
    except Exception as e:
        print(f"❌ Unexpected error: {e}")
    finally:
        connections.disconnect(alias=alias)

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: python load_collection.py <collection_name>")
        sys.exit(1)

    host_name = sys.argv[1]
    collection_name = sys.argv[2]
    load_collection(host_name, collection_name)
```
