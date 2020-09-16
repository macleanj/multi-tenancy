# Overview
The custom directory is the only directory that is managed outside the repository [fork](https://github.com/kubernetes-sigs/multi-tenancy). For changes outside the custom directory a PR should be opened to the original project. Alternatively the files will be overwritten the next time the origin is pulled.

# Preparation
[Compile kubectl-mtb](../benchmarks/kubectl-mtb/README.md)

# Testing
## Cleaning namespaces
The default testing namespaces are:
- tenant-a-prdev
- tenant-b-prdev

Make sure the environment is clean before start. Otherwise you might hit the resource quota limit and the test will fail.
```
kubectl delete all --all -n tenant-a-prdev
kubectl delete all --all -n tenant-b-prdev
```

## Running the test
The testing description can be found in the [original](../benchmarks/README.md) project.:
```
cd ../benchmarks/kubectl-mtb
kubectl-mtb run benchmarks -n "tenant-a-prdev" --as "tenant-admin" -d
```

By default all tests will be executed. To make the set of tests more manageable during development the ```--skip``` option can be used.

# Cleanup
After testing the tested namespaces should be left in a clean state to to consume resource unnecessary.

# Kubernetes Commands
An overview of commands that can come in handy in addition to the [Kubernetes Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

Namespaces and cluster resources
```
kubectl api-resources --namespaced=false
```

Convert YAML to JSON
```
yq r k8s_manifest.yml -j -P
```