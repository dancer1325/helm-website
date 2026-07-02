---
title: "Custom Resource Definitions"
description: "How to handle creating and using CRDs."
weight: 7
aliases: ["/docs/topics/chart_best_practices/custom_resource_definitions/"]
---

* goal
  * how to create & use CRD objects

* MAIN components | work with CRDs
  * ".yaml" / declare the CRD 

    ```yaml
    kind: CustomResourceDefinition
    ```
  * resources / use the CRD
    * requirements
      * ⚠️install the CRD⚠️

## ways to install the CRD

Helm is optimized to load as many resources into Kubernetes as fast as possible.
By design, Kubernetes can take an entire set of manifests and bring them all
online (this is called the reconciliation loop).

But there's a difference with CRDs.

For a CRD, the declaration must be registered before any resources of that CRDs
kind(s) can be used
* And the registration process sometimes takes a few seconds.

### Method 1: -- by -- `helm`

* `crd-install` hooks
  * | Helm v2
    * Reason:🧠| Helm v3, deprecated🧠

* steps
  * | your chart's "/crds",
    * place your ".yaml" CRD definitions
      * ❌are NOT templated❌
  * `helm install`
    * install the CRDs
      * if you want to skip the installation -> `helm install --skip-crds` 
      * if the CRD ALREADY exists -> it will be skipped with a warning

#### Some caveats (and explanations)

Another important point to consider in the discussion around CRD support is how
the rendering of templates is handled
* One of the distinct disadvantages of the
`crd-install` method used in Helm 2 was the inability to properly validate
charts due to changing API availability (a CRD is actually adding another
available API to your Kubernetes cluster)
* If a chart installed a CRD, `helm` no
longer had a valid set of API versions to work against
* This is also the reason
behind removing templating support from CRDs
* With the new `crds` method of CRD
installation, we now ensure that `helm` has completely valid information about
the current state of the cluster.

* `helm` commands / 
  * ❌NOT supported by CRDs❌
    * `helm upgrade` / `helm uninstall`
      * == you can NOT update or eliminate CRDs
      * Reason:🧠 avoid unintentional data loss🧠
    * `helm install --dry-run`
      * Reason:🧠
        * `--dry-run`
          * 's goal: validate that chart's output will actually work | sent to the server
          * Helm can NOT install -- , via `--dry-run`, -- the CRD 
        * CRDs
          * == modification -- of the -- server's behavior🧠
  * 👀supported by CRDs👀
    * `helm template`
    * [separate chars](#method-2-place-crd--separated-charts)

### Method 2: place CRD | SEPARATED Charts

* steps
  * | a chart,
    * place the CRD definition 
  * | another chart,
    * place any resources / use that CRD

* requirements
  * install EACH chart SEPARATELY

* audience
  * cluster operators / have admin access to a cluster
