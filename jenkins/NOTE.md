
## Uninstall and cleanup

```
oc delete all,pvc -l app=jenkins-persistent
```

## Install

```bash
oc import-image jenkins:2 --confirm --scheduled=true \
  --from=registry.access.redhat.com/openshift3/jenkins-2-rhel7:v3.11

oc new-app --template=jenkins-persistent \
  -p MEMORY_LIMIT=2Gi \
  -p VOLUME_CAPACITY=10Gi \
  -p NAMESPACE= \
  -e INSTALL_PLUGINS=permissive-script-security:0.5,script-security:1.60,update-sites-manager:2.0.0,timestamper:1.9,ssh-agent:1.17,email-ext:2.66,ownership:0.12.1,http_request:1.8.22,run-selector:1.0.0 \
  -e JENKINS_JAVA_OVERRIDES=-Dpermissive-script-security.enabled=no_security

# or
oc process jenkins-persistent -p MEMORY_LIMIT=2Gi -p VOLUME_CAPACITY=10Gi -p NAMESPACE= | oc apply -f -
oc set env dc/jenkins INSTALL_PLUGINS=permissive-script-security:0.5,script-security:1.60,update-sites-manager:2.0.0,timestamper:1.9,ssh-agent:1.17,email-ext:2.66,ownership:0.12.1,http_request:1.8.22,run-selector:1.0.0 JENKINS_JAVA_OVERRIDES=-Dpermissive-script-security.enabled=no_security
```
