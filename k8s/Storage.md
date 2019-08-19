# Storage

## Configure a volume to store these logs at /var/log/webapp on the host 

```bash
---
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - name: event-simulator
    image: simulator
    env:
    - name: LOG_HANDLERS
      value: file
    volumeMounts:
    - mountPath: /log
      name: log-volume

  volumes:
  - name: log-volume
    hostPath:
      path: /var/log/webapp
      type: Directory
      
 ```
 
 ## Configure a persistent volume
 
 ```bash
      
---
apiVersion: v1
kind: PersistentVolume
metadata:
   name: pv-log
spec:
  accessModes:
      - ReadWriteMany  capacity:
  storage: 100Mi  
  hostPath:
     path: /pv/log
     type: Directory


 ```
 
 ## Configure a persistent volume Claim
 
 ```bash
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Mi
      

 ```
 
 ## use the same persistent volume claim
 
 ```bash
---
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - name: event-simulator
    image: simulator
    env:
    - name: LOG_HANDLERS
      value: file
    volumeMounts:
    - mountPath: /log
      name: log-volume

  volumes:
  - name: log-volume
    persistentVolumeClaim:
      claimName: claim-log-1
      
 ```
 
