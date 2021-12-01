# ConfigMap
A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:
- Inside a container command and args.
- Environment variables for a container.
- Add a file in read-only volume, for the application to read.
- Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap.

Check Out kubernetes documentation [link-01](https://kubernetes.io/docs/concepts/configuration/configmap) [link-02](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap)

## Deploy Examples
- Example 01
```bash
kubectl apply -f example-01.yaml
kubectl logs pod/test-pod-00
```
Output
```bash
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
var_name_01=value_00
```
- Example 02
```bash
kubectl apply -f example-02.yaml
kubectl logs pod/test-pod-02
```
Output
```bash
KUBERNETES_PORT=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT=443
HOSTNAME=test-pod-02
SHLVL=1
HOME=/root
key_01=value_01
key_02=value_02
KUBERNETES_PORT_443_TCP_ADDR=10.152.183.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP=tcp://10.152.183.1:443
KUBERNETES_SERVICE_PORT_HTTPS=443
PWD=/
KUBERNETES_SERVICE_HOST=10.152.183.1
```
- Example 03
```bash
kubectl apply -f example-03.yaml
kubectl logs pod/test-pod-03
```
Output
```bash
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
var_name_03=value_03
var_name_04=value_04
```
- Example 04
```bash
kubectl apply -f example-04.yaml
kubectl logs pod/test-pod-04
```
Output
```bash
line-01 on file-00.txt
line-02 on file-00.txt
line-03 on file-00.txt
```
- Example 05
```bash
kubectl apply -f example-05.yaml
kubectl logs pod/test-pod-05
```
Output
```bash
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

