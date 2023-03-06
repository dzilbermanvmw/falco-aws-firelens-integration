# Multi-cluster security with Falco and FluentBit on Amazon EKS & ECS

This repository hold all source code needed for the blogpost about [Multi-cluster
security with Falco and FluentBit on Amazon EKS & ECS](https://sysdig.com/blog/multi-cluster-security-firelens/).

There are one directory per step or infrastructure fondation to automate:

* `ecs`: Deploy Falco and FluentBit on ECS
* `eks`: Deploy Falco and FluentBit on EKS

## Getting started

You will need the following requisites:

* `Helm`  deployed on the EKS cluster
* `aws cli` tools to handle all AWS configuration settings. Ensure you are using
  an aws cli tools which uses boto greater or equal to 1.12.224
* `jq` to help with the scripts
* `kubectl` to test deployment of EKS based components

### Deploying EKS integration

Ensure your worker nodes can send logs to CloudWatch. We automated this step with
the Makefile but you will need to edit and set the `NODE_ROLE_NAME` value to your
own settings. Then:

```
$ cd eks
$ make
```

This will create and attach the EKS-CloudWatchLogs policy to your node IAM role
to make sure you can send logs to CloudWatch and will deploy FluentBit daemonset
with the CloudWatch output plugin.

You can deploy Falco using corresponding [Helm Chart](https://github.com/falcosecurity/falco.git).

### Deploying ECS integration

Like EKS, we automated this step with a Makefile. You will need to edit and set
the `CLUSTER_ARN` and `NODE_ROLE_NAME` as well. Then:

```
$ cd ecs
$ make
```

And this will create and attach the ECS-CloudWatchLogs policy to your node IAM
role to ensure containers can send logs to CloudWatch, create a task which
deploys Falco on ECS with an attached sidecar container for FluentBit which
sends the logs to CloudWatch.
