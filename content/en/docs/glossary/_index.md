---
title: "Glossary" 
description: "Terms used to describe components of Helm's architecture."
weight: 9
---

# Glossary

## Chart

* := helm package / contains Kubernetes resources                 -- Check 'charts/*/templates' --
  * allows
    * with its information — being installed in — Kubernetes cluster     -- Follow README.md --
  * content 👁️ in a well-defined directory structure 👁️
    * `Chart.yaml`
    * `templates/`
    * `values.yaml`
    * dependencies                  -- Check 'charts/withdependencies' --
  * — packaged into a — Chart Archive   -- Check 'charts/hello-world' --

## Chart Archive

* == chart /            -- Check 'charts/hello-world' --
  * tarred
  * gzipped
  * signed optionally

## Chart Dependency (Subcharts)

* == Chart dependency      
* if you package a chart (`helm package`) → all chart’s dependencies are bundled with it   -- Check 'charts/withdependencies' --
* types
  * soft          -- Check 'charts/softsubchart' --
    * := chart can work / without being installed previously the other chart
    * NO tools provided by helm
  * hard          -- Check 'charts/withdependencies' --
    * := chart / — depends on — another chart
      * 👁️must be placed under `charts/` 👁️
      * once the chart is installed → all the dependencies are installed
    * chart + chart’s dependencies — are managed as — a collection

## Chart Version

* follows https://semver.org/   -- Check 'Chart.yaml' -- 

## Chart.yaml
* MANDATORY / EVERY chart

## helm
* CLI

## Helm Configuration Files (XDG)

* := directories / store helm’s configuration files
  * first time `helm`  is run → they are created
* Check '../FrequentQuestions/ChangeHelmv2vsv3'

## Kube Config (KUBECONFIG)
* TODO:
The Helm client learns about Kubernetes clusters by using files in the _Kube
config_ file format. By default, Helm attempts to find this file in the place
where `kubectl` creates it (`$HOME/.kube/config`).

## Lint (Linting)

* == validate that the chart follows
  * conventions &  -- Check '../BestPractices' --
  * requirements of the standard
* `helm lint`  -- Check ''../HelmCommands' --

## Provenance (Provenance file)

* `.prov`        -- Check '../Topics/Helm Provenance and Integrity' -- 
* == cryptographic hash of Chart Archive + Chart.yaml data + signature block
  * == validate
    * where the chart comes from
    * what it contains
* optional
* can be served from
  * chart repository server or
  * any HTTP server

## Release

* := Running instance of a chart + specific config
* once `helm install` → it’s created              -- Check 'helm-examples' repo --
  * different release name / installation
* allows
  * tracking the installation

## Release Number (Release Version)
* allows                                          -- Check 'helm-examples' repo --
  * tracking the different releases’ changes

## Rollback

A release can be upgraded to a newer chart or configuration. But since release
history is stored, a release can also be _rolled back_ to a previous release
number. This is done with the `helm rollback` command.

Importantly, a rolled back release will receive a new release number.

| Operation  | Release Number                                       |
|------------|------------------------------------------------------|
| install    | release 1                                            |
| upgrade    | release 2                                            |
| upgrade    | release 3                                            |
| rollback 1 | release 4 (but running the same config as release 1) |

The above table illustrates how release numbers increment across install,
upgrade, and rollback.

## Helm Library (or SDK)
* https://pkg.go.dev/helm.sh/helm/v3

## Repository (Repo, Chart Repository)

* := HTTP servers /
  * store packaged helm charts                             -- Check 'helm-examples' repo --
  * serves an `index.yaml`                        
    * /
      * describes a batch of charts
      * path to downloads the charts
    * — can be created via — `helm repo index`
    * _Example:_ https://github.com/artifacthub/hub/blob/master/docs/chart/index.yaml
* helm clients — can point to — several chart repositories

## Chart Registry (OCI-based Registry)
* := Storage & Distribution system for Helm chart packages
  * OCI-based
* can contain ≥ 0 Chart Repository

## Values (Values Files, values.yaml)
* == configuration settings for the charts
* 'Values.yaml'
  * default config
* ways to pass customized values
  * `-f otherValue.yaml`
  * `--set key=value`
