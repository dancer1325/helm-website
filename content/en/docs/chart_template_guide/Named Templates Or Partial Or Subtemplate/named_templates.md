---
title: "Named Templates"
description: "How to define named templates."
weight: 9
---

* goal
  * about _named templates_,
    * how to define them | file,
    * how to use

* _named template_
  * OR _partial_ OR _subtemplate_
  * 👀:= template / defined | file + given name👀
    * ⚠️template names are global⚠️
      * -> 👀if you declare 2 templates / SAME name -> LAST loaded will be the one used👀
      * recommendations
        * 👀name your templates /
          * _chart-specific names_👀
            * == `chartName-*`  — prefix —
            * Reason: 🧠 templates | subcharts + top-level templates -- are -- compiled together 🧠
            ```
            {{ define "chartName.namedTemplateToGive" }}
            ```
          * _chart-version-specific names_👀
            * Reason: 🧠SAME as before🧠
            ```
            {{ define "chartName.versionChart.namedTemplateToGive" }}
            ```
  * ways to access them
    * by name EVERYWHERE

* functions -- to -- manage named templates
  * `define`
  * `template`
  * `block`
  * `include`
    * ⚠️ONLY AVAILABLE | named templates⚠️
    * 's behavior == `template`'s behavior

## Partials & `_` files

* TODO: 
Before we get to the nuts-and-bolts of writing those templates, there is file
naming convention that deserves mention:

* Most files in `templates/` are treated as if they contain Kubernetes manifests
* The `NOTES.txt` is one exception
* But files whose name begins with an underscore (`_`) are assumed to _not_ have
  a manifest inside. These files are not rendered to Kubernetes object
  definitions, but are available everywhere within other chart templates for
  use.

These files are used to store partials and helpers. In fact, when we first
created `mychart`, we saw a file called `_helpers.tpl`. That file is the default
location for template partials.

## Declaring and using templates -- via -- `define` & `template`

* `define`
  * == action /
    * 👀create a named template | template file👀

    ```yaml
    {{- define "MY.NAME" }}
      # body of template here
    {{- end }}
    ```
  * uses
    * encapsulate a Kubernetes block of labels
  * ❌by itself, NOT produce output❌
  * by convention,
    * it should have a SIMPLE documentation block / describe it
      ```
      {{/* ... */}}
      ```

* `template`
  * == action /
    * embed named templates | other files (TODO: only valid | Kubernetes manifests ❓)
      * 👀== output `define` 👀

* how does the template engine reads the file / has `define` & `template`?
  * if it finds `define` -> store away the reference
  * once it finds `template` -> that reference is called -> render that template inline

TODO: create a helm chart & check it
```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: running-panda-configmap
  labels:
    generator: helm
    date: 2016-11-02
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "pizza"
```

Conventionally, Helm charts put these templates inside of a partials file,
usually `_helpers.tpl`. Let's move this function there:

```yaml
{{/* Generate basic labels */}}
{{- define "mychart.labels" }}
  labels:
    generator: helm
    date: {{ now | htmlDate }}
{{- end }}
```

Even though this definition is in `_helpers.tpl`, it can still be accessed in
`configmap.yaml`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  {{- template "mychart.labels" }}
data:
  myvalue: "Hello World"
  {{- range $key, $val := .Values.favorite }}
  {{ $key }}: {{ $val | quote }}
  {{- end }}
```

As mentioned above, **template names are global**. As a result of this, if two
templates are declared with the same name the last occurrence will be the one
that is used. Since templates in subcharts are compiled together with top-level
templates, it is best to name your templates with _chart specific names_. A
popular naming convention is to prefix each defined template with the name of
the chart: `{{ define "mychart.labels" }}`.

## Setting the scope of a template

In the template we defined above, we did not use any objects. We just used
functions. Let's modify our defined template to include the chart name and chart
version:

```yaml
{{/* Generate basic labels */}}
{{- define "mychart.labels" }}
  labels:
    generator: helm
    date: {{ now | htmlDate }}
    chart: {{ .Chart.Name }}
    version: {{ .Chart.Version }}
{{- end }}
```

If we render this, we will get an error like this:

```console
$ helm install --dry-run moldy-jaguar ./mychart
Error: unable to build kubernetes objects from release manifest: error validating "": error validating data: [unknown object type "nil" in ConfigMap.metadata.labels.chart, unknown object type "nil" in ConfigMap.metadata.labels.version]
```

To see what rendered, re-run with `--disable-openapi-validation`:
`helm install --dry-run --disable-openapi-validation moldy-jaguar ./mychart`.
The result will not be what we expect:

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: moldy-jaguar-configmap
  labels:
    generator: helm
    date: 2021-03-06
    chart:
    version:
```

What happened to the name and version? They weren't in the scope for our defined
template. When a named template (created with `define`) is rendered, it will
receive the scope passed in by the `template` call. In our example, we included
the template like this:

```yaml
{{- template "mychart.labels" }}
```

No scope was passed in, so within the template we cannot access anything in `.`.
This is easy enough to fix, though. We simply pass a scope to the template:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  {{- template "mychart.labels" . }}
```

Note that we pass `.` at the end of the `template` call. We could just as easily
pass `.Values` or `.Values.favorite` or whatever scope we want. But what we want
is the top-level scope.

Now when we execute this template with `helm install --dry-run --debug
plinking-anaco ./mychart`, we get this:

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: plinking-anaco-configmap
  labels:
    generator: helm
    date: 2021-03-06
    chart: mychart
    version: 0.1.0
```

Now `{{ .Chart.Name }}` resolves to `mychart`, and `{{ .Chart.Version }}`
resolves to `0.1.0`.

## The `include` function

Say we've defined a simple template that looks like this:

```yaml
{{- define "mychart.app" -}}
app_name: {{ .Chart.Name }}
app_version: "{{ .Chart.Version }}"
{{- end -}}
```

Now say I want to insert this both into the `labels:` section of my template,
and also the `data:` section:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  labels:
    {{ template "mychart.app" . }}
data:
  myvalue: "Hello World"
  {{- range $key, $val := .Values.favorite }}
  {{ $key }}: {{ $val | quote }}
  {{- end }}
{{ template "mychart.app" . }}
```

If we render this, we will get an error like this:

```console
$ helm install --dry-run measly-whippet ./mychart
Error: unable to build kubernetes objects from release manifest: error validating "": error validating data: [ValidationError(ConfigMap): unknown field "app_name" in io.k8s.api.core.v1.ConfigMap, ValidationError(ConfigMap): unknown field "app_version" in io.k8s.api.core.v1.ConfigMap]
```

To see what rendered, re-run with `--disable-openapi-validation`:
`helm install --dry-run --disable-openapi-validation measly-whippet ./mychart`.
The output will not be what we expect:

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: measly-whippet-configmap
  labels:
    app_name: mychart
app_version: "0.1.0"
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "pizza"
app_name: mychart
app_version: "0.1.0"
```

Note that the indentation on `app_version` is wrong in both places. Why? Because
the template that is substituted in has the text aligned to the left. Because
`template` is an action, and not a function, there is no way to pass the output
of a `template` call to other functions; the data is simply inserted inline.

To work around this case, Helm provides an alternative to `template` that will
import the contents of a template into the present pipeline where it can be
passed along to other functions in the pipeline.

Here's the example above, corrected to use `indent` to indent the `mychart.app`
template correctly:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
  labels:
{{ include "mychart.app" . | indent 4 }}
data:
  myvalue: "Hello World"
  {{- range $key, $val := .Values.favorite }}
  {{ $key }}: {{ $val | quote }}
  {{- end }}
{{ include "mychart.app" . | indent 2 }}
```

Now the produced YAML is correctly indented for each section:

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: edgy-mole-configmap
  labels:
    app_name: mychart
    app_version: "0.1.0"
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "pizza"
  app_name: mychart
  app_version: "0.1.0"
```

> It is considered preferable to use `include` over `template` in Helm templates
> simply so that the output formatting can be handled better for YAML documents.

Sometimes we want to import content, but not as templates. That is, we want to
import files verbatim. We can achieve this by accessing files through the
`.Files` object described in the next section.
