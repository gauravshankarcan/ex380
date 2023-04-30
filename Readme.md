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

oc get deployment cluster-samples-operator -n openshift-cluster-samples-operator -o jsonpath='{.status.availableReplicas}'
oc get pod --all-namespaces -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName

oc get oauth cluster -o json

secret_name=$(oc get oauth cluster \
  -o jsonpath="{.spec.identityProviders[$filter].htpasswd.fileData.name}")
oc extract secret/$secret_name -n openshift-config --confirm
oc get pod -n openshift-authentication -o name
```

```bash
curl -k --header "Authorization: Bearer $TOKEN" -X GET https://api.example.com:6443/api
/openapi/v2 
```
```yaml
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: file-integrity-operator (name should match )
  namespace: openshift-file-integrity
spec:
  targetNamespaces:
  - openshift-file-integrity (name should match )
  ```

```
oc get clusterversion version -o jsonpath='{.status.desired.image}'
oc adm release extract --to=release-image --from=quay.io/openshift-release-dev/ocp-release@sha256:7ffe...cc56
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
