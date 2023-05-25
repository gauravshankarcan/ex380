
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
    
    
## QUAY
```env
config.yaml -- secret 

AUTHENTICATION_TYPE: LDAP
FEATURE_USER_INITIALIZE: true
BROWSER_API_CALLS_XHR_ONLY: false
SUPER_USERS:
- cloudadmin
FEATURE_USER_CREATION: true
LDAP_ADMIN_DN: uid=admin,cn=users,cn=accounts,dc=ocp4,dc=example,dc=com
LDAP_ADMIN_PASSWD: Redhat123@!
LDAP_ALLOW_INSECURE_FALLBACK: false
LDAP_BASE_DN:
    - cn=accounts
    - dc=ocp4
    - dc=example
    - dc=com
LDAP_EMAIL_ATTR: mail
LDAP_UID_ATTR: uid
LDAP_URI: ldaps://idm.ocp4.example.com
LDAP_USER_RDN:
    - cn=users
FEATURE_TEAM_SYNCING: true
TEAM_RESYNC_STALE_TIME: 60m
FEATURE_NONSUPERUSER_TEAM_SYNCING_SETUP: true
```

## Image restrict 
```yaml
apiVersion: config.openshift.io/v1
kind: Image  1
metadata:
  annotations:
    release.openshift.io/create-only: "true"
  creationTimestamp: "2022-05-17T13:44:26Z"
  generation: 1
  name: cluster 2
  resourceVersion: "8302"
  selfLink: /apis/config.openshift.io/v1/images/cluster
  uid: e34555da-78a9-11e9-b92b-06d6c7da38dc
spec:
  additionalTrustedCA: 3
    name: myconfigmap
  registrySources: 4
    allowedRegistries: 5
    - example.com
    - quay.io
    - registry.redhat.io
    - image-registry.openshift-image-registry.svc:5000
    - reg1.io/myrepo/myapp:latest
    insecureRegistries: 6
    - insecure.com
```    

```shell
oc process \
   -f template-app-deployment.yaml \
   -p "APPLICATION=invoices-app" \
   | oc create -n invoices-app -f -
   
podman push localhost/hello:1 \
  central-quay-registry.apps.ocp4.example.com/finance/budget-app-dev:1   
```

## Stackrox
```yaml

apiVersion: platform.stackrox.io/v1alpha1
kind: SecuredCluster
metadata:
  name: stackrox-secured-cluster-services
  namespace: stackrox
spec:
  auditLogs:
    collection: Auto
  admissionControl:
    listenOnUpdates: true
    bypass: BreakGlassAnnotation
    contactImageScanners: DoNotScanInline
    listenOnCreates: true
    timeoutSeconds: 3
    listenOnEvents: true
  scanner:
    analyzer:
      scaling:
        autoScaling: Enabled
        maxReplicas: 5
        minReplicas: 2
        replicas: 3
    scannerComponent: AutoSense
  perNode:
    collector:
      collection: KernelModule
      imageFlavor: Regular
    taintToleration: TolerateTaints
  clusterName: managed-cluster
  centralEndpoint: 'central-stackrox.apps.ocp4.example.com:443'
```
