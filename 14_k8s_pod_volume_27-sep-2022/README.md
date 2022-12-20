## Container orchestration Kubernetes basics: containers, pods, ephemeral volumes 27 Sep 2022 ##
------------------------------------------------------------------------------------------------

Before you begin, please comply the following **Cleaning actions**

```
k3d cluster delete demo-cluster-1
k3d cluster delete demo-cluster-2
```

Please follow the steps of 
[Prerequisites](https://github.com/Alliedium/springboot-api-rest-example/tree/master/.k8s#1-prerequisites), 
[Example 1](https://github.com/Alliedium/springboot-api-rest-example/blob/master/.k8s/01-single-pod-with-ephemeral-volume), 
and 
[Example 2](https://github.com/Alliedium/springboot-api-rest-example/blob/master/.k8s/02-pods-with-ephemeral-volume) 
from ```.k8s``` folder of the project 
[Alliedium/springboot-api-rest-example](https://github.com/Alliedium/springboot-api-rest-example/) 
