# OpenShift Pipelines / Tekton Experiments

## Prerequisites

* OpenShift 4.3+ cluster (or CodeReady Containers based on at OpenShift 4.3+)
* OpenShift Pipelines Operator installed in the cluster

## Notes

Currently based on `v1alpha1` Tekton spec. This will be upgraded once OpenShift Pipelines moves to `v1beta1` spec.

## Prepping Example Envioronment

This first cut expects a `cicd` namespace with Nexus and SonarQube installed and configured, as well as a Maven `settings.xml` file in a ConfigMap to tie it all together.  This can be setup with a nice little one-liner:

```
$ oc apply -k infra
```

To run the pipelines in this repo, you will also need `dev` and `uat` environments for the petclinic app, as well as the associated RoleBindings to allow the `petclinic` service account in each project to pull images from the `cicd` project.  The `pipeline` service account in the `cicd` projet also required the `admin` role on the `dev` and `uat` projects in order to start rollouts.  This done with another one-liner:

```
$ oc apply -k app/overlays/all
```

The pipeline resources (tasks, resoruces, pipelines, etc...) can be created with:

```
$ oc apply -k tekton
```

You can then start a pipeline build with:
```
$ oc create -f pipelinerun/run-build.yaml
```
or
```
# Works, but you can't push commits to my repo, so you'll need to fork my app repo and substitute
# your own git creds in the git credentials secret.
$ oc create -f pipelinerun/run-release.yaml
```
 

