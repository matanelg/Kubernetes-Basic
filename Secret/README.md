# Secret
A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.
Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.
To use a Secret, a Pod needs to reference the Secret. A Secret can be used with a Pod in three ways:
- As files in a volume mounted on one or more of its containers.
- As container environment variable.
- By the kubelet when pulling images for the Pod.
Check Out kubernetes documentation [link-01](https://kubernetes.io/docs/concepts/configuration/secret/) [link-02](https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-kubectl/) [link-03](https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-config-file/)


## Commands
```bash
kubectl apply -f <secret.yaml>
kubectl get secret
kubectl get secret <secret-name> -o yaml
kubectl describe secret <secret-name>

kubectl create secret <secret-name> \
  --from-file=<secret-file-path>  \
  --from-file=<secret-file-path> 

kubectl create secret generic <secret-name> \
  --from-file=<secret-key>=<secret-file-path> \
  --from-file=password=<secret-file-path> 

kubectl create secret generic <secret-name> \
  --from-literal=<key>=<value> \
  --from-literal=<key1>=<value1>
```

## Examples
- Example-01 - Create Secret from yaml.
```bash
echo -n 'admin-01' | base64
echo -n 'password-01' | base64
echo -n 'admin-02' | base64
echo -n 'password-02' | base64

cat <<EOF > secret-01.yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-01
type: Opaque
data:
  username-01: YWRtaW4tMDE=
  password-01: cGFzc3dvcmQtMDE=
stringData:
  config.yaml: |
    username-02: YWRtaW4tMDI=
    password-02: cGFzc3dvcmQtMDI=
EOF
kubectl apply -f secret-01.yaml
kubectl describe secrets secret-01
```
Output
```diff
YWRtaW4tMDE=
cGFzc3dvcmQtMDE=
YWRtaW4tMDI=
cGFzc3dvcmQtMDI=
secret/secret-01 created
Name:         secret-01
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
config.yaml:  56 bytes
password-01:  11 bytes
username-01:  8 bytes

```

- Example-02 - create Secret with kubectl (no need to encode).
```bash
# create from file.
echo -n 'admin' > ./username.txt
echo -n 'password' > ./password.txt
kubectl create secret generic secret-02 \
  --from-file=username=./username.txt \
  --from-file=password=./password.txt
kubectl describe secrets secret-02
```
Output
```diff
secret/secret-02 created
Name:         secret-02
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
password:  8 bytes
username:  5 bytes
```

```bash
# create from literal.
kubectl create secret generic secret-03 \
  --from-literal=username=user \
  --from-literal=password=password
kubectl describe secrets secret-03
```
Output
```diff
secret/secret-03 created
Name:         secret-03
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
password:  8 bytes
username:  4 bytes

```

- Example-03 -Put Secret in container as an environment variable.
```bash
echo -n 'admin-01' | base64
echo -n 'password-01' | base64
kubectl apply -f secret-04.yaml
kubectl describe secrets secret-04
kubectl logs pod/secret-env-pod
```
Output
```diff
YWRtaW4tMDE=
cGFzc3dvcmQtMDE=
secret/secret-04 created
pod/secret-env-pod created
Name:         secret-04
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
password-01:  11 bytes
username-01:  8 bytes
username=admin-01
password=password-01
```

## Terminology
```yaml
---
apiVersion: v1                        # apiVersion - Kubernetes default api version.
kind: Secret                          # Kind - Create Secret object.
metadata:                             # metadata - Here we can define object name, labels and annotaions.
  name: secret-04                     # name - Object name.
type: Opaque                          # type - Opaque is the default Secret type.
data:                                 # data - Store key value pairs & files that will insert to pod/container.
  username-01: YWRtaW4tMDE=           # Create key value pair, value have to encode with base64.
  password-01: cGFzc3dvcmQtMDE=

---
apiVersion: v1
kind: Pod                             # Kind - Create Pod object.
metadata:
  name: secret-env-pod
spec:                                 # spec - How to manage the pod.
  containers:                         # containers - Create containers.
  - name: mycontainer                 # name - Container name
    image: k8s.gcr.io/busybox         # image - Container base image.
    command: ["/bin/sh", "-c"]        # command - Run command when the container is up.
    args:                             # args - Run multiple commands when the container is up.
      - env | grep username;
        env | grep password;
    env:                              # env - Create environment variables on the container.
      - name: username                # name - The name of the environment variable.
        valueFrom:                    # valueFrom - From where to take variable value.
          secretKeyRef:               # secretKeyRef - Take variable value from Secret object.
            name: secret-04           # name - The name of the secret that store the variable.
            key: username-01          # key - The key name of the value in the secret.
      - name: password
        valueFrom:
          secretKeyRef:
            name: secret-04 
            key: password-01
  restartPolicy: Never                # restartPolicy - we dont wanna restart the container after its finish to run.
```

