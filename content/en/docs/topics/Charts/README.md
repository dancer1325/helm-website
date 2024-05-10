
# `chart.yaml`
* Check '/chartSkeleton' -- `helm create chartSkeleton` -- 

# Chart dependencies
* TODO:
## `alias`
* 👁️optional field to declare a chart dependency 👁️
* uses
  * access a chart with another name
* _Example:_ Add to 'chartSkeleton'
  * Add chart's dependency nginx from bitnami, adding an alias
  * Use that alias in the parent to look for it -- Check 'NOTES.txt'
  * `helm dependency update`
  * `helm install skeleton .`
    * Problems: "chartName"
      * Solution: Rename the parent chart
## manage manually via “charts/”
* == copy the dependant unpacked charts under “charts/”
  * `helm pull -- untar`
