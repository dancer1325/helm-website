- Objects
    - can contain other
        - objects OR
        - functions
- Release
    ```
    Release
      Name
      Namespace
      IsUpgrade
      IsInstall
      Revision
      Service
    ```
  - `IsUpgrade`
    - := boolean / specifies if the operation is an Upgrade Or Rollback
  - `IsInstall`
    - := boolean / specifies if the operation is an `helm instal`
  - `Revision`
    - := number of the release
      - on install → 1
      - +1 / each upgrade OR rollback
  - `Service`
    - := service / rendering the template
      - = `Helm` always
- `Values`
    - == values passed in to the template from —
        - ‘values.yaml’ &
        - user-supplied files
    - by default, empty
- `Charts`
    - == contents of 'chart.yaml'
- Subcharts
    ```
    Subcharts
      Release
      Values
      Charts
       ...
    ```
  - == all subchart’s built-in objects (Release, Values, Charts, …)
- Files
    ```
    Files
      Get
      GetBytes
      Glob
      Lines
      AsSecrets
      AsConfig
    ```
  - allows
    - accessing to all NON-special files in a chart
  - `Get fileName`
    - == get a file — by — name
  - `GetBytes`
    - == get the contents of a file — as — []byte
  - `Glob pattern`
    - == get a list of files / match the pattern
  - `Lines`
    - == read a file / line by line
  - `AsSecrets`
    - == file body — as — Base64 String
  - `AsConfig`
    - == file body — as — yaml map
- Capabilities
    ```
    Capabilities
      APIVersions
        .Has $version
      KubeVersion
        Version
        Major
        Minor
      HelmVersion
        Version
        GitCommit
        GitTreeState
        GoVersion
    ```
  - allows
    - accessing capabilties of Kubernetes cluster
  - `HelmVersion`
    - == `helm version` details
    - `.GoVersion`
      - == Go compiler version
- Template
    ```
    Template
      Name
      BasePath
    ```
  - allows
    - getting information about the current template

## Examples
* Check 'builtinobject' helm chart, the built-in objects included
* How was created?
  * `helm create builtinobject`
* `helm upgrade built .`
  * each time you want to rerun
* TODO: Add ` {{ .Files.* }}`