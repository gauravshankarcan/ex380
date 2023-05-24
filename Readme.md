
## Appilcations
Kustomize - image transfer / nameSuffix namePrefix
```yaml
images:
  - name: "(.*)"
    newName: "MATCHED"
    newTag: "fake"
    
kind: Kustomization

bases:
  - ../../base

resources:
- dbclaim-pvc.yaml    
```
    
    
