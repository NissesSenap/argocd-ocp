# ArgoCD in openshift

This is repo is a oponioated way of installing and managing a openshift 4 cluster using ArgoCD.

My intent is that the repo should include everything that comes after your initial installation using the ArgoCD [App Of Apps Pattern](https://argoproj.github.io/argo-cd/operator-manual/cluster-bootstrapping/#app-of-apps-pattern)

The plan is that you should be able to use this repo even if you are running OKD and none Openshift kubernetes,
so I will try to use as few of the internal openshift crd:s as possible to make it as general as possible.

When I'm unable to use general API I will try to put them in a seperate argocd deployment pipeline.
For example handeling the setup of all operators will be done in one repo/folder.
This will enable the developer to create a PR to the "platform" team asking them to install a opeator/crd.
When the PR is merged they will be able to use that operator without the need of "platform" team doing anything.
It will also make it easy for the users of this repo to opt in or out of specific features.

Here is an example of Operations that will be included in the repo:

- Setting the internal-repository storage
- Setting up the EFK stack inside the cluster
- Setting up basic prometheus scanning of external apps.

Long run:

- Setup basic infrastrucutre to have application development
  - Static code analzys tool like Sonarqube
  - Some security scanning tool
- Setup basic pipeline using tetcton

## Install ArgoCD

This part should be create like an ansible role or soemthing but for now i will do it manually (I know writing manuall in a ArgoCD repo is wrong :D).

I currently want to get started with ArgoCD and I don't care to badly about long-term operations even if that is the goal of the repo. My current plan to manage access is to [follow](https://blog.openshift.com/openshift-authentication-integration-with-argocd/) and use dex.

### Setup

#### The OCP way

Open OLM UI and install the ArgoCD operator.

```shell
oc new-project argocd

oc create -f argocd/ArgoCD.yml

# If you wan't to give argocd service account cluster-admin
oc apply -f argocd/cluster-admin.yml

```

## App of Apps

My plan is to use helm overall in this repo, if someone else want to use kustomize or what ever templating language,
please feel free to do so. We will put it in another subfolder and it should be fine.
The more exampels the better.

You can use argocd to setup the sync but it's usefull to have when testing your helm chart.

I'm counting that you already have installed helm v3, if not see [here](https://helm.sh/docs/intro/install/)

### Setup app of apps

argocd app create apps \
    --dest-namespace argocd \
    --dest-server https://kubernetes.default.svc \
    --repo https://github.com/NissesSenap/argocd-ocp.git \
    --path app_of_apps/helm  
argocd app sync apps  
