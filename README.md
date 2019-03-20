# freebyJenkinsAgent - A Jenkins JNLP Agent with kubectl, helm, and helm push support

This repository contains the docker and Jenkins build files for the container that is used as a special agent for the Jenkins container inside the Kubernetes cluster. It gives helm and kubectl support and support to also push charts to a chart museum.

The version of helm and kubernetes defined in the Jenkinsfile should match your cluster, those can be determined with these simple commands:
`


# Debugging

* If your cluster is locked down and you do not have sufficient rights to be able perform services like list Pods you will
*  receive an error like this when attempting to perform that service with kubectl:

 `
 (Forbidden): User "system:serviceaccount:somenamespace:default" cannot list pods in the namespace "somenamespace".
 `
 
 You need to make sure either the default service acccount in the namespace in which Jenkins is running has enough rights to administer the whole cluster `--clusterrole cluster-admin` or has sufficient rights to perform the operations you are looking to perform. For more information please
