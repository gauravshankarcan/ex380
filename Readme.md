
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
FEATURE_USER_INITIALIZE: true 1
BROWSER_API_CALLS_XHR_ONLY: false
SUPER_USERS:
- quayadmin
FEATURE_USER_CREATION: true
AUTHENTICATION_TYPE: LDAP 2
LDAP_ADMIN_DN: uid=admin,cn=users,cn=accounts,dc=ocp4,dc=example,dc=com 3
LDAP_ADMIN_PASSWD: Q@&fh3eR5
LDAP_ALLOW_INSECURE_FALLBACK: false
LDAP_BASE_DN:
    - cn=accounts
    - dc=ocp4
    - dc=example
    - dc=com
LDAP_EMAIL_ATTR: mail
LDAP_UID_ATTR: uid
LDAP_URI: ldap://myldap.ocp4.example.com
FEATURE_TEAM_SYNCING: true 4
```
