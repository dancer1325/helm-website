- := template / defined inside a file + given name
    - name
        - ⚠️are GLOBAL⚠️
            - == if template1Name == template2Name → template2 is used
            - → conventions
                - `chartName-*`  — prefix —
                - `chartName.chartVersion*`  — prefix —
    - allows
        - — being accessed by — name elsewhere

- conventions
    - all files under ‘/templates’ → Kubernetes manifests
      - **Note:** Except to ‘Notes.txt’
    - if fileName `_*`
        - →
            - NO Kubernetes manifests
            - available everywhere
        - uses
            - storing
                - partials
                - helpers
- TODO: