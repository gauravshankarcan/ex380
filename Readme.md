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
    
resources:
- deployment.yaml   1
secretGenerator:
- name: mycert   2
  files:
  - tls.crt=priv-cert.crt   3
  - tls.key=priv-cert.key
  type: kubernetes.io/tls   4
generatorOptions:
  disableNameSuffixHash: true 
  
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


```
pipeline {
  triggers {
    pollSCM ('H/3 * * * *')  / cron ('H/5 * * * *')
  }
  options {
    disableConcurrentBuilds()
  }
  
  agent {
    node {
      label 'nodejs'
    }
  }
  
          sh 'oc diff -k config | tee drift-report.txt'
        sh '! test -s drift-report.txt'
   sh 'oc apply --dry-run --validate -k config'
   
    stage ('Verify test user') {
      when {
        branch 'main'
      }   
      
      
  post {
    failure {
      archiveArtifacts artifacts: '*.txt'
      build job: 'apply/main'
    }
    success {
      sh 'rm drift-report.txt'
      sh 'echo \'There is no configuration drift\' > no-drift.txt'
      archiveArtifacts artifacts: '*.txt'
    }
    
```

```console
oc create secret generic ldap-secret -n openshift-config --from-literal=bindPassword=${LDAP_ADMIN_PASSWORD}
wget -c -nv http://idm.example.com/ipa/config/ca.crt
oc create configmap ca-config-map  -n openshift-config --from-file=ca.crt

   ldap:
      attributes:  1
        id:
        - dn
        email:
        - mail
        name:
        - cn
        preferredUsername:
        - uid
      bindDN: "uid=admin,cn=users,cn=accounts,dc=ocp4,dc=example,dc=com"  2
      bindPassword:  3
        name: ldap-secret
      ca:  4
        name: ca-config-map
      insecure: false
      url: "ldaps://idm.ocp4.example.com/cn=users,cn=accounts,dc=ocp4,dc=example,dc=com?uid" 
```
 
 ```yaml
oc adm groups sync --sync-config path --confirm

kind: LDAPSyncConfig
apiVersion: v1
url: ldaps://idm.ocp4.example.com
bindDN: uid=admin,cn=users,cn=accounts,dc=example,dc=com
bindPassword:
  file: /etc/secrets/bindPassword
insecure: false
ca: /etc/config/ca.crt
rfc2307:
    groupsQuery:
        baseDN: "cn=groups,cn=accounts,dc=example,dc=com"
        scope: sub
        derefAliases: never
        pageSize: 0
        filter: (objectClass=ipausergroup)
    groupUIDAttribute: dn
    groupNameAttributes: [ cn ]
    groupMembershipAttributes: [ member ]
    usersQuery:
        baseDN: "cn=users,cn=accounts,dc=example,dc=com"
        scope: sub
        derefAliases: never
        pageSize: 0
    userUIDAttribute: dn
    userNameAttributes: [ uid ]
``` 

```console
oc patch proxy/cluster --type=merge  --patch='{"spec":{"trustedCA":{"name":"<CONFIGMAP-NAME>"}}}'
oc patch ingresscontroller.operator/default 
oc patcg apiserver 

cat WILDCARD.pem CA.pem > COMBINED-CERT.pem

openssl x509 -in wildcard-api.pem -text
```
```yaml
config.openshift.io/inject-trusted-cabundle=true

        volumeMounts:
        - mountPath: /etc/pki/ca-trust/extracted/pem
          name: trusted-ca
          readOnly: true
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - configMap:
          defaultMode: 420
          items:
          - key: ca-bundle.crt
            path: tls-ca-bundle.pem
          name: <CONFIGMAP-NAME>
        name: trusted-ca
```

```console
oc adm cordon worker06
oc adm drain worker06 -delete-emptydir-data --ignore-daemonsets --force --disable-eviction

 oc adm new-project debug --node-selector=""
```

```yaml
storageclass.kubernetes.io/is-default-class=true


...output omitted...
      volumeDevices: 1
        - name: data 2
          devicePath: /dev/xvda 3
  volumes:
    - name: data 4
      persistentVolumeClaim:
        claimName: block-pvc 5
        
        
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: iscsi-blk  1
provisioner: kubernetes.io/no-provisioner  2
allowVolumeExpansion: false       
        
        IN SS
      storageClassName: iscsi-blk
      accessModes: [ "ReadWriteOnce" ]        
        
```
```console
[student@workstation PVs]$ limit1="requests.storage=6G"
[student@workstation PVs]$ limit2="persistentvolumeclaims=2"
[student@workstation PVs]$ sclass="iscsi-blk.storageclass.storage.k8s.io"
[student@workstation PVs]$ limit3="${sclass}/persistentvolumeclaims=1"
[student@workstation PVs]$ oc create quota storage \
  --hard=${limit1},${limit2},${limit3}


<storage-class-name>.​storageclass.storage.k8s.io/​requests.storage
```  

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-pv-01
spec:
  capacity:
    storage: 500Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: local-storage
  local:  1
    path: /mnt/disks/vol1  2

  
apiVersion: "local.storage.openshift.io/v1"  1
kind: "LocalVolume"  2
metadata:
  name: "local-disks"  3
spec:
  storageClassDevices:
    - storageClassName: "fast-local-storage"  4
      volumeMode: Filesystem
      devicePaths:
        - /dev/sdb  5
    - storageClassName: "standard-local-storage"  6
      volumeMode: Filesystem
      devicePaths:
        - /dev/hda 
 ```       
