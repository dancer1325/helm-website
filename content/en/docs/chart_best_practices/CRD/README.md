- Goal
    - best practices to
        - create CRD &
        - use CRD

- Main 2 pieces
    - `.yaml` / define the CRD 
    `….
    kind: CustomResourceDefinition
    …`
        - 👁️must be done first & can take some seconds 👁️
    - other resources / use the CRD

- Ways to install the CRD, before using it
    - via helm
        - steps
            - place your `.yaml` defining the CRD under `/crds` in your chart
            - `helm install`
        - considerations
            - `helm upgrade` & `helm install --dry-run` NOT supported for CRDs
                - → alternatives
                    - `helm template`
                    - via separate charts -- next option --
    - via separate charts
        - uses
            - cluster operators / admin rights