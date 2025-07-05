# Volumes - Persisting Application Data

## 3 k8s components for storage:
- persistent volume
- persistent volume claim
- storage class

## The Need of Volumes
kubernetes doesn't provide data persistence out-of-the-box, you need to configure it

**Requirements:**
- storage must not depend on the pod lifecycle (because pods are ephemeral)
- storage must be available on all nodes
- storage needs to survive even if cluster crashes

## Persistent Volume
Think in a Persistent Volume as a cluster resource just like RAM and CPU.

- **created via yaml:**  
    - `kind: PersistentVolume`  
- needs actual physical storage

    - **tricky part:**  
        - Where does this storage come from and who makes it available to the cluster?
    - What type of storage do you need?

    - The storage management is out of kubernetes' scope. It must be done outside the cluster, and the PersistentVolume component is just an interface to the actual storage.

    - You can think of storage as an external plugin to your cluster.

- **Persistent Volumes are NOT namespaced**

## Local vs. Remote Volume Types
Local volume types violate requirements 2 and 3 for data persistence:
- tied to one specific node
- not surviving a cluster crash

`Not used for DataBase persistence.` Use remote storage instead.

## Who Creates Persistent Volumes
There are two roles:
- **k8s admin**
- **k8s user**

**Storage resource is provisioned by the admin. And the user makes their application claim the Persistent Volume.**

Persistent Volume Claim is also a component  
`kind: PersistentVolumeClaim`

**Summing up:**
1. admins configure storage
2. admins create Persistent Volumes
3. users claim Persistent Volumes through PersistentVolumeClaim
4. users make their pod's use the PVC in the Pod's spec (note: the Pod and the PVC must be in the same namespace)

## Level of Volume Abstractions
- Pod requests the volume through PersistentVolumeClaim
- Claim tries to find a volume in the cluster (note: claims must be in the same namespace as the pod)
- Volume has the actual storage backend info

## ConfigMap and Secret
ConfigMap and Secret are local volumes, managed by kubernetes. They can be mounted into your pod/container.

## Storage Class
Storage Class is used when there are hundreds of pods requesting hundreds of Persistent Volumes.

**StorageClass provisions Persistent Volumes dynamically, when PersistentVolumeClaim claims it.**

Another abstraction level, abstracting:
- underlying storage provider
- parameters for that storage

**Storage Class is also a kubernetes component, created via yaml file:**  
- `kind: StorageClass`  
- backend defined via 
`provisioner` attribute

**Usage:**
1. pod claims a volume via PVC
2. PVC requests storage from StorageClass
3. StorageClass creates a PersistentVolume that meets the needs of the claim (using the defined provisioner)

**My words:** basically Storage Class is a way to automatically create Persistent Volumes when a Pod claims one.