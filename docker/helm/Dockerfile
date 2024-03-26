FROM openjdk:8

ARG HELM_VERSION=3.14.3
ARG HELM_HOME=/opt/helm/

RUN apt-get -y update && apt-get install -y wget gnupg curl

#Install Helm
RUN wget https://get.helm.sh/helm-v${HELM_VERSION}-linux-amd64.tar.gz
RUN tar -xzf helm-v${HELM_VERSION}-linux-amd64.tar.gz
RUN mv linux-amd64/ ${HELM_HOME}
RUN rm -r helm-v${HELM_VERSION}-linux-amd64.tar.gz

ENV PATH="$HELM_HOME:${PATH}"