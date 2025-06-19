---
title: "Built-in Objects"
description: "Built-in objects available to templates."
weight: 3
---

* Objects
  * uses
    * FROM template engine, are passed -- into a -- template
  * ways to create NEW objects | your templates
    * `tuple` function
  * can contain
    * 1 value
    * OTHER objects or functions
      * _Examples:_ `Release`, `Files`

# `Release`
- allows
  - describing the release itself
- `Release.Name`
- `Release.Namespace`
  - if the manifest does NOT override -> namespace | be released into
- `Release.IsUpgrade`
  - if the current operation == upgrade or rollback -> `true` 
- `Release.IsInstall`
  - if the current operation == install -> `true` 
- `Release.Revision`
  - == release's revision number
  - | install, == 1
  - +1 / EAC upgrade OR rollback
- `Release.Service`
  - service / render the template
  - | Helm, (ALWAYS) == `Helm`

# `Values`
* == values passed | template
  * 👀-- from the -- `values.yaml` file + user-supplied files👀
  * by default, empty

# `Chart`
* TODO: The contents of the `Chart.yaml` file. Any data in `Chart.yaml` will
  be accessible here. For example `{{ .Chart.Name }}-{{ .Chart.Version }}` will
  print out the `mychart-0.1.0`.
  - The available fields are listed in the [Charts Guide]({{< ref
    "/docs/topics/charts.md#the-chartyaml-file" >}})

# `Subcharts`
This provides access to the scope (.Values, .Charts, .Releases etc.)
  of subcharts to the parent. For example `.Subcharts.mySubChart.myValue` to access
  the `myValue` in the `mySubChart` chart.

# `Files`
This provides access to all non-special files in a chart. While you
  cannot use it to access templates, you can use it to access other files in the
  chart. See the section [Accessing Files]({{< ref
    "/docs/chart_template_guide/accessing_files.md" >}}) for more.
  - `Files.Get` is a function for getting a file by name (`.Files.Get
    config.ini`)
  - `Files.GetBytes` is a function for getting the contents of a file as an
    array of bytes instead of as a string. This is useful for things like
    images.
  - `Files.Glob` is a function that returns a list of files whose names match
    the given shell glob pattern.
  - `Files.Lines` is a function that reads a file line-by-line. This is useful
    for iterating over each line in a file.
  - `Files.AsSecrets` is a function that returns the file bodies as Base 64
    encoded strings.
  - `Files.AsConfig` is a function that returns file bodies as a YAML map.

# `Capabilities`
This provides information about what capabilities the
  Kubernetes cluster supports.
  - `Capabilities.APIVersions` is a set of versions.
  - `Capabilities.APIVersions.Has $version` indicates whether a version (e.g.,
    `batch/v1`) or resource (e.g., `apps/v1/Deployment`) is available on the
    cluster.
  - `Capabilities.KubeVersion` and `Capabilities.KubeVersion.Version` is the
    Kubernetes version.
  - `Capabilities.KubeVersion.Major` is the Kubernetes major version.
  - `Capabilities.KubeVersion.Minor` is the Kubernetes minor version.
  - `Capabilities.HelmVersion` is the object containing the Helm Version details, it is the same output of `helm version`.
  - `Capabilities.HelmVersion.Version` is the current Helm version in semver format.
  - `Capabilities.HelmVersion.GitCommit` is the Helm git sha1.
  - `Capabilities.HelmVersion.GitTreeState` is the state of the Helm git tree.
  - `Capabilities.HelmVersion.GoVersion` is the version of the Go compiler used.

# `Template`
* Contains information about the current template that is being
  executed
  - `Template.Name`: A namespaced file path to the current template (e.g.
    `mychart/templates/mytemplate.yaml`)
  - `Template.BasePath`: The namespaced path to the templates directory of the current
    chart (e.g. `mychart/templates`).

The built-in values always begin with a capital letter. This is in keeping with
Go's naming convention. When you create your own names, you are free to use a
convention that suits your team. Some teams, like many whose charts you may see
on [Artifact Hub](https://artifacthub.io/packages/search?kind=0), choose to use
only initial lower case letters in order to distinguish local names from those
built-in. In this guide, we follow that convention.
