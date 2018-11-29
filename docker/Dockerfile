# 
#   Builds a special Jenkins Slave image that has kubectl and helm binaries already installed
#   and configured for Chart Museum access.
#    
#   Authors: James Eby
#
#   This container expects certain environment variables to be passed to it either through
#   the -e flag or the --env-file flag. A default env-file flag has been created in this
#   directory and simply exports all the values from the host OS to the runtime container
#   environment through a command like this:
#
#   docker run --env-file env.list 
#

FROM docker:18.06

# Version of the kubernetes server as found by running
# "kubectl version" on your cluster
# Note: Latest version of kubectl may be found at:
# https://aur.archlinux.org/packages/kubectl-bin/
ENV KUBE_LATEST_VERSION="v1.11.3"

# Version of the Helm Client and Tiller as found by running
# "helm version" on your cluster
# Note: Latest version of helm may be found at:
# https://github.com/kubernetes/helm/releases
ENV HELM_VERSION="v2.9.1"

RUN apk add --no-cache ca-certificates bash git \
    && wget -q https://storage.googleapis.com/kubernetes-release/release/${KUBE_LATEST_VERSION}/bin/linux/amd64/kubectl -O /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl \
    && wget -q https://storage.googleapis.com/kubernetes-helm/helm-${HELM_VERSION}-linux-amd64.tar.gz -O - | tar -xzO linux-amd64/helm > /usr/local/bin/helm \
    && chmod +x /usr/local/bin/helm

RUN helm init --client-only \
    && helm plugin install https://github.com/chartmuseum/helm-push \
    && helm repo add --username ${REGISTRY_USER} --password ${REGISTRY_USER_PASSWORD} freebytech https://${REGISTRY_URL}/chartrepo/freebytech
