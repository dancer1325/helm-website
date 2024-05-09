# Chart
* := helm package / contains Kubernetes resources                 -- Check 'charts/*/templates' --
  * allows
    * with its information — being installed in — Kubernetes cluster     -- Follow README.md --
  * content 👁️ in a well-defined directory structure 👁️
    * `Chart.yaml`
    * `templates/`
    * `values.yaml`
    * dependencies                  -- Check 'charts/withdependencies' --
  * — packaged into a — Chart Archive   -- Check 'charts/hello-world' --

# Chart Archive
* == chart /            -- Check 'charts/hello-world' --
  * tarred
  * gzipped
  * signed optionally

# Subcharts
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

# Chart Version
* follows https://semver.org/   -- Check 'Chart.yaml' -- 

# `helm`
* CLI 

# XDG
* := directories / store helm’s configuration files
  * first time `helm`  is run → they are created
* Check '../FrequentQuestions/ChangeHelmv2vsv3'

# Lint
* == validate that the chart follows
  * conventions &  -- Check '../BestPractices' --
  * requirements of the standard
* `helm lint`  -- Check ''../HelmCommands' --

# Provenance file
* `.prov`        -- Check '../Topics/Helm Provenance and Integrity' -- 
* == cryptographic hash of Chart Archive + Chart.yaml data + signature block
  * == validate
    * where the chart comes from
    * what it contains
* optional
* can be served from
  * chart repository server or
  * any HTTP server

# Release
* := Running instance of a chart + specific config
* once `helm install` → it’s created              -- Check 'helm-examples' repo --
  * different release name / installation
* allows
  * tracking the installation

# Release Number
* allows                                          -- Check 'helm-examples' repo --
  * tracking the different releases’ changes

# Helm library / SDK
* https://pkg.go.dev/helm.sh/helm/v3

# Helm client
* CLI

# Chart Repository / Repo
* := HTTP servers which
  * store packaged helm charts                             -- Check 'helm-examples' repo --
  * serves an `index.yaml`                       -- _Example:_ https://github.com/artifacthub/hub/blob/master/docs/chart/index.yaml -- 
    * /
      * describes a batch of charts
      * path to downloads the charts
    * — can be created via — `helm repo index`
* helm clients — can point to — several chart repositories

# Chart Registry 
* := Storage & Distribution system for Helm chart packages
  * OCI-based
* can contain ≥ 0 Chart Repository

# Values
* == configuration settings for the charts
* 'Values.yaml'
  * default config
* ways to pass customized values
  * `-f otherValue.yaml`
  * `--set key=value`

# Notes
* Chart of example can be found under the repo 'helm-examples'
