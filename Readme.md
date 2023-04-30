```
oc new-app --template jenkins-persistent
oc adm policy add-cluster-role-to-user self-provisioner -z jenkins -n gitops-deploy

GIT_SSL_CAINFO
```

```yaml
Kustomize

resources:
- deployment.yaml
secretGenerator:
- name: mycert
  namespace: openshift-config
  files:
  - tls.crt=priv-cert.crt  
  - tls.key=priv-cert.key
  type: kubernetes.io/tls 
generatorOptions:
  disableNameSuffixHash: true
  

secretGenerator:
- name: htpasswd-secret
  namespace: openshift-config
  files:
  - htpasswd=htpasswd-secret-data  
  ```
