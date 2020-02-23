# Logging

If you look [in](https://docs.openshift.com/container-platform/4.3/logging/cluster-logging-deploying.html)
you will see "generateName: "elasticsearch-"" in the Example subscription.

This can't be used in ArgoCD at the time of writing these helm charts.
For more [info](https://github.com/argoproj/argo-cd/issues/1639)
