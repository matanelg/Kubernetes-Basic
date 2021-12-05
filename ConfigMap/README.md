# ConfigMap
A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:
- Inside a container command and args.
- Environment variables for a container.
- Add a file in read-only volume, for the application to read.
- Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap.

Check Out kubernetes documentation [link-01](https://kubernetes.io/docs/concepts/configuration/configmap) [link-02](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap)

## Commands
```diff
kubectl create configmap <map-name> <data-source>	# create config map from data source.
kubectl get configmaps					# get all config maps on namespace.
kubectl get configmaps <map-name> -o yaml		# get config map object as yaml file.
kubectl describe configmaps <map-name>                  # get data store on config map object.

kubectl create configmap <map-name> --from-file=<file-or-directory>
kubectl create configmap <map-name> --from-file=<file-or-directory-01> --from-file=<file-or-directory-02>
kubectl create configmap <map-name> --from-env-file=<file-with-env-vars>
kubectl create <map-name> \
        --from-env-file=<file-with-env-vars-01> \
        --from-env-file=<file-with-env-vars-02>
kubectl create configmap <map-name> --from-file=<my-key-name>=<path-to-file>
kubectl create configmap <map-name> --from-literal=<key>=<value> --from-literal=<key>=<value>
```

## Yaml Examples
- Example 01 - Create environment variable on container.
```bash
kubectl apply -f example-01.yaml
kubectl logs pod/test-pod-00
```
Output
```diff
KUBERNETES_PORT=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=test-pod-00
SHLVL=1
HOME=/root
KUBERNETES_PORT_443_TCP_ADDR=10.152.183.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
PWD=/
KUBERNETES_SERVICE_HOST=10.152.183.1
+ var_name_01=value_00
```
- Example 02 - Create all environment variables from the confing map using envFrom.
```bash
kubectl apply -f example-02.yaml
kubectl logs pod/test-pod-02
```
Output
```diff
KUBERNETES_PORT=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=test-pod-02
SHLVL=1
HOME=/root
+ key_01=value_01
+ key_02=value_02
KUBERNETES_PORT_443_TCP_ADDR=10.152.183.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
PWD=/
KUBERNETES_SERVICE_HOST=10.152.183.1
```
- Example 03 - Create environment variables from two separate confing maps.
```bash
kubectl apply -f example-03.yaml
kubectl logs pod/test-pod-03
```
Output
```diff
KUBERNETES_PORT=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=test-pod-03
SHLVL=1
HOME=/root
KUBERNETES_PORT_443_TCP_ADDR=10.152.183.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
PWD=/
KUBERNETES_SERVICE_HOST=10.152.183.1
+ var_name_03=value_03
+ var_name_04=value_04
```
- Example 04 - Mount file from config map to container. 
```bash
kubectl apply -f example-04.yaml
kubectl logs pod/test-pod-04
```
Output
```diff
line-01 on file-00.txt
line-02 on file-00.txt
line-03 on file-00.txt
```
- Example 05 - Mount files $ create environment variables on container.
```bash
kubectl apply -f example-05.yaml
kubectl logs pod/test-pod-05
```
Output
```diff
print env:
var_name_01=value_02
var_name_02=value_03
check /etc/config path:
total 12
drwxrwxrwx    3 root     root          4096 Dec  1 21:01 .
drwxr-xr-x    1 root     root          4096 Dec  1 21:01 ..
drwxr-xr-x    2 root     root          4096 Dec  1 21:01 ..2021_12_01_21_01_16.295128454
lrwxrwxrwx    1 root     root            31 Dec  1 21:01 ..data -> ..2021_12_01_21_01_16.295128454
lrwxrwxrwx    1 root     root            18 Dec  1 21:01 file-02.txt -> ..data/file-02.txt
lrwxrwxrwx    1 root     root            18 Dec  1 21:01 file-03.txt -> ..data/file-03.txt
cat files in /etc/config/:
line-01 on file-02.txt
line-02 on file-02.txt    
line-01 on file-03.txt
line-02 on file-03.txt
line-03 on file-03.txt  
```

## Terminology
```yaml
---
apiVersion: v1                                   # apiVersion - Kubernetes default api version.
kind: ConfigMap                                  # Kind - Create ConfigMap object.
metadata:                                        # metadata - Here we can define object name, labels and annotaions.  
  name: multiple-data                            # name - Object name.
data:                                            # data - Store key value pairs & files that will insert to pod/container. 
  key_02: "value_02"                             # Create key value pairs.
  key_03: "value_03"                             # Create key value pairs.
  file-02.txt: |                                 # Create file.
    line-01 on file-02.txt
    line-02 on file-02.txt
  file-03.txt: |                                 # Create file.
    line-01 on file-03.txt
    line-02 on file-03.txt
    line-03 on file-03.txt   

---
apiVersion: v1                                   # apiVersion - Kubernetes default api version.
kind: Pod                                        # Kind - Create Pod object.
metadata:                                        # metadata - Here we can define object name, labels and annotaions.
  name: test-pod-05                              # name - Object name.
spec:                                            # spec - How to manage the pod.
  containers:                                    # containers - Create containers.
    - name: test-container-05                    # name - Container name.
      image: k8s.gcr.io/busybox                  # image - Container base image.
      command: ["/bin/sh", "-c"]                 # command - Run command when the container is up.
      args:                                      # args - Run multiple commands when the container is up.
        - echo "print env:";
          env | grep var_name_01;
          echo "var_name_02=$(var_name_02)";
          echo "check /etc/config path:";
          ls -la /etc/config;
          echo "cat files in /etc/config/:";
          cat /etc/config/file-02.txt;
          cat /etc/config/file-03.txt;
      env:                                       # env - Create environment variables on the container.
        - name: var_name_01                      # name - The name of the environment variable.
          valueFrom:                             # valueFrom - From where to take variable value.
            configMapKeyRef:                     # configMapKeyRef - Take variable value from ConfigMap object.
              name: multiple-data                # name - The name of the config map that store the variable.
              key: key_02                        # key - The key name of the value in the config map.
        - name: var_name_02
          valueFrom:
            configMapKeyRef:
              name: multiple-data
              key: key_03      
      volumeMounts:                             # volumeMounts - Mount associated volume with pod into the container. 
      - name: config-volume-01                  # name - volume name.
        mountPath: /etc/config                  # mountPath - Mount to file system path in the container.
  volumes:                                      # volumes - Create a volume that store ConfigMap object.
    - name: config-volume-01                    # name - Volume name.
      configMap:                                # configMap - Define that we wanna associate ConfigMap object.
        name: multiple-data                     # name - The name of the config map.
        items:                                  # items - we can choose what we wanna to maount.
        - key: file-02.txt                      # key - the data we want to maount.
          path: file-02.txt                     # path - the name of the file in the container.
        - key: file-03.txt
          path: file-03.txt        
  restartPolicy: Never                          # restartPolicy - we dont wanna restart the container after its finish to run.
```

## Create with kubectl Examples
- Create configmap for files from directory.
```bash
kubectl create configmap configmap-example-01 --from-file=files/ -o yaml
kubectl describe configmaps configmap-example-01
```
Output
```diff
Name:         configmap-example-01
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
env-01.txt:
----
env_01_key_01=value_01

env-02.txt:
----
env_02_key_01=value_01
env_02_key_02=value_02

file-01.txt:
----
line-01

file-02.txt:
----
line-01
line-02

Events:  <none>

```

- Create configmap from two separate files.
```bash
kubectl create configmap configmap-example-02 --from-file=files/file-01.txt --from-file=files/file-02.txt -o yaml
kubectl describe configmaps configmap-example-02
```
Output
```diff
Name:         configmap-example-02
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
file-01.txt:
----
line-01

file-02.txt:
----
line-01
line-02

Events:  <none>

```

- Create configmap with environment variables from two separate files.
```bash
kubectl create configmap configmap-example-03 \
               --from-env-file=files/env-01.env \
               --from-env-file=files/env-01.env -o yaml
kubectl describe configmaps configmap-example-03
```
Output
```diff
Name:         configmap-example-03
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
env_01_key_01:
----
value_01
Events:  <none>

```

- Create configmap with spesific key for file.
```bash
kubectl create configmap configmap-example-04 --from-file=file-01=files/file-01.txt -o yaml
kubectl describe configmaps configmap-example-04
```
Output
```diff
Name:         configmap-example-04
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
file-01:
----
line-01

Events:  <none>

```

- Create configmap with environment variables from literal.
```bash
kubectl create configmap configmap-example-05 --from-literal=key01=value01 --from-literal=key02=value02 -o yaml
kubectl describe configmaps configmap-example-05
```
Output
```diff
Name:         configmap-example-05
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
key02:
----
value02
key01:
----
value01
Events:  <none>
```
