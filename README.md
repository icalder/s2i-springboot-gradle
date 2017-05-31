# OpenShift S2I Builder for Spring Boot + Gradle

## Obtain s2 tools
https://github.com/openshift/source-to-image/releases/tag/v1.1.6

## Create docker image
cd s2i-springboot-gradle  
make

## Create ImageStream in OpenShift
oc create -f springboot-gradle-s2i-is.yml

```
apiVersion: v1
kind: ImageStream
metadata:
  annotations:
    openshift.io/display-name: Spring Boot Gradle Project
  name: springboot-gradle-s2i
spec:
  tags:
  - annotations:
      description: Builder for Spring Boot Gradle projects.  Project must contain
        Gradle wrapper.
      iconClass: icon-openjdk
      openshift.io/display-name: Spring Boot Gradle Project
      sampleRepo: https://github.com/icalder/osdemo.git
      supports: java
      tags: builder,java
      version: "1.0"
    generation: 6
    importPolicy: {}
    name: "1.0"
    referencePolicy:
      type: Source
  - annotations: null
    from:
      kind: DockerImage
      name: 172.30.1.1:5000/openshift/springboot-gradle-s2i:1.0
    generation: null
    importPolicy: {}
    name: latest
    referencePolicy:
      type: ""

```

## Tag and push to OpenShift internal registry
```
docker tag springboot-gradle-s2i:latest 172.30.1.1:5000/openshift/springboot-gradle-s2i:1.0
docker login -u admin -p $(oc whoami -t) 172.30.1.1:5000
docker push 172.30.1.1:5000/openshift/springboot-gradle-s2i:1.0
```

