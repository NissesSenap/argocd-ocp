# OCS

Openshift Storage solution building on CEPH, Rook and noobaa.

I enjoy this [blog:](https://blog.openshift.com/ocs-4-2-in-ocp-4-2-14-upi-installation-in-rhv/) that goes through how to setup OCS on RHV.

It also shows you how to use local-storage operator if you don't have a good storage class that you can use.

I'm currently running my environment on a vmware vsphere environment, so I won't follow this perfectly but I use it for insperation.

## Tag nodes

You need to tag your nodes to be able to run OCS on them.
This is **NOT** handeld in helm chart. This due to that node config isn't easy to handle due to a bunch of unic data
that is inserted in to it.

In the mean time you will have to do the following manually, i know it sucks.

Your nodes dosen't need to be seperated in to other seperate racks but it's a good idea :).

Change to match your node/domain names.

```shell
oc label node worker-1.ocp42.ssa.mbu.labs.redhat.com "cluster.ocs.openshift.io/openshift-storage=" --overwrite
oc label node worker-1.ocp42.ssa.mbu.labs.redhat.com "topology.rook.io/rack=rack0" --overwrite
oc label node worker-2.ocp42.ssa.mbu.labs.redhat.com "cluster.ocs.openshift.io/openshift-storage=" --overwrite
oc label node worker-2.ocp42.ssa.mbu.labs.redhat.com "topology.rook.io/rack=rack1" --overwrite
oc label node worker-3.ocp42.ssa.mbu.labs.redhat.com "cluster.ocs.openshift.io/openshift-storage=" --overwrite
oc label node worker-3.ocp42.ssa.mbu.labs.redhat.com "topology.rook.io/rack=rack3" --overwrite
```

## Taint nodes for prod

According to the [RedHat documentation](https://access.redhat.com/documentation/en-us/red_hat_openshift_container_storage/4.2/html/deploying_openshift_container_storage/deploying-openshift-container-storage)
you shoulden't run any other workloads on your OCS nodes.

"You can create application pods either on OpenShift Container Storage nodes or non OpenShift Container Storage nodes and run your applications. However, it is recommended that you apply a taint to the nodes to mark them for exclusive OpenShift Container Storage use and not run your applications pods on these nodes.
Because the tainted OpenShift nodes are dedicated to storage pods, they will only require a OpenShift Container Storage subscription and not a OpenShift subscription"

In the link they talk about how to taint the nodes.
This will proably make your life easier when you run in production especially if you are running  a bigger OCS env.

You can find how to add the node taints in the documentation.

I won't take this in to account when I setup my env.

## Size

OCS requieres crazy big nodes by default.
Since I don't run this in production I lower the requierments, if you wan't to have the normal requierments [update storagecluster.yaml](helm/templates/storagecluster.yaml)
