# Overview
The custom directory is the only directory that is managed outside the repository fork. For changes outside the custom directory a PR should be opened to the original project. Alternatively the files will be overwritten the next time the origin is pulled.

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

## Preparing kubeconfig
Generate and copy the following kubeconfig files into the ./custom directory:
- kubeconfig-${K8S_CLUSTER_ENV}_cluster-admin.yml
- kubeconfig-${K8S_CLUSTER_ENV}_tenant-a-prdev-tenant-admin.yml
- kubeconfig-${K8S_CLUSTER_ENV}_tenant-b-prdev-tenant-admin.yml
> K8S_CLUSTER_ENV is the Managed K8S Cluster environment

## Running the test
The testing description can be found in the [original](../benchmarks/documentation/run.md) project. For our multi-tenant environment we will only focus on PL1 as we will not implement additional CustomResourceDefinitions (CRD):
```
cd ../benchmarks
go test -v ./e2e/tests -config ../../../custom/config-${K8S_CLUSTER_ENV}.yaml -ginkgo.focus PL1
```
> K8S_CLUSTER_ENV is the Managed K8S Cluster environment

By default all tests will be executed. To make the set of tests more manageable during development the file ```./benchmarks/e2e/tests/e2e.go``` will control which set of tests will be executed. Just comments out all tests and comment in only the one that you need to focus on.

# Cleanup
After testing the tested namespaces should be left in a clean state to to consume resource unnecessary.

# Commands
An overview of commands that can come in handy in addition to the [Kubernetes Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

Namespaces and cluster resources
```
kubectl api-resources --namespaced=false
```

Convert YAML to JSON
```
yq r k8s_manifest.yml -j -P
```