# Helm allows
* managing Charts
  * creating from scratch
  * installing / uninstalling     -- `helm install` / `helm uninstall` --
  * versioning              -- `helm upgrade` --
  * …
* — packaging into → Chart Archive          -- `helm package` --
* — interacting with — chart repositories   -- `helm repo` --

# Main concepts
* Chart
* Configuration      -- Check 'helm-examples/#Values'
* Release            -- Check 'helm-examples/#Release'

# Components / Architecture
* helm client
* helm library
* helm config         -- Check 'helm-examples/#Helm config'
  * := Kubernetes Secrets about the releases

# Notes
* Chart of example can be found under the repo 'helm-examples'
