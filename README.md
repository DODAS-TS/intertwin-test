# InterTwin APIs example

In this repository you can find and end2end implementation of a virtual kubelet payload distribution via InterTwins APIs

You will:
- instantiate a VirtualKubelet (vk) deployment on an existing Kubernetes (YOU MUST HAVE ONE FOR YOUR OWN ALREADY)
- On a machine outside the kubernetes cluster you can the deploy the remote part of the interTwin kubelete (API server + resource plugin)
- The resource plugin used in this example is a simple (mock) one based on Docker, just to show an example of successful flow

The example workflow consists in the following steps:
- the user creates a pod indicating a particular node selector, pointing toward the vk node
- the vk kubelet will digest the request and it will delegate the lifecycle management of the container(s) to the remote server
- you will eventually see a docker container running on the remote machine while the k8s cluster will show the pod as RUNNING

## Demo requirements

- K8s cluster 1.20+
- A VM with port 3000 and 4000 open and docker daemon installed and running

## Deploy the virtual kubelet

```bash
git clone https://github.com/DODAS-TS/intertwin-test.git

cd intertwin-test/vk-kustomization
```

In `./InterLinkConfig.yaml` substitute the urls with the ones of your remote machine, then apply the kustomization manifest with:

```bash

kubectl create ns vk

kubectl apply -n vk -k ./
```

If everything went well when the pod will be running you will also see a nee node appearing with `kubectl get node`


## Deploy the Intertwin API components on the remote server

```bash
git clone https://github.com/DODAS-TS/intertwin-test.git

cd intertwin-test/

docker compose up -d
```

## Deploy a demo pod targetting the vk node

You can now run a demo pod on the vk node with:

```bash
cd intertwin-test/
kubectl apply -f ./payloads/busyecho_k8s.yaml
```
