
# springboot-gradle-s2i
FROM openshift/base-centos7

MAINTAINER Iain Calder <iain.calder@gmail.com>

ENV SPRING_BOOT_GRADLE_BUILDER_VERSION 1.0

# TODO: Set labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Platform for building Spring Boot Gradle apps" \
  io.k8s.display-name="builder spring-boot-gradle" \
  io.openshift.expose-services="8080:http" \
  io.openshift.tags="builder,spring-boot,gradle"

RUN yum install -y tar java-1.8.0-openjdk-devel && yum clean all -y

# TODO (optional): Copy the builder files into /opt/app-root
# COPY ./<builder_folder>/ /opt/app-root/

# Copy the S2I scripts to /usr/libexec/s2i, since openshift/base-centos7 image sets io.openshift.s2i.scripts-url label that way, or update that label
COPY usage assemble save-artifacts run /usr/libexec/s2i/

RUN chown -R 1001:1001 /opt

# This default user is created in the openshift/base-centos7 image
USER 1001

EXPOSE 8080

CMD ["usage"]
