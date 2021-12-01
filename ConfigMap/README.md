# ConfigMap
A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.<br/>
There are four different ways that you can use a ConfigMap to configure a container inside a Pod:
- Inside a container command and args.
- Environment variables for a container.
- Add a file in read-only volume, for the application to read.
- Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap.<br/>
Check Out kubernetes documentation [link-01](https://kubernetes.io/docs/concepts/configuration/configmap) [link-02](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap)

