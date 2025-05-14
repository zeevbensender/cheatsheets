# MILVUS
## Python Scripts

### Load Collection
```python
import sys
from pymilvus import connections, Collection, exceptions

def load_collection(host_name, collection_name, port="19530", alias="default"):
    try:
        print(f"Connecting to Milvus at {host_name}:{port}...")
        connections.connect(alias=alias, name=host), port=port)

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
    if not (host_name or collection_name):
        print("host name and collection name must be provided")
        exit(0)
    load_collection(host_name, collection_name)
```
