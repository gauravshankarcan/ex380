```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: k8s-deploy-prod
bases:
  - ../../base
resources:
  - deployment.yaml  
images:
  - name: registry.ocp4.example.com:8443/redhattraining/versioned-hello
    newTag: v1.1
```
```console
kubectl apply -k .
oc whoami -t

oc import-image registry.ocp4.example.com:8443/developer/versioned-hello:latest --confirm --scheduled
  
oc set triggers deployment/hello --from-image versioned-hello:latest -c hello  
```


```zsh
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
