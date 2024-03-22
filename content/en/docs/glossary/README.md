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



# Notes
* Chart of example can be found under the repo 'helm-examples/'
