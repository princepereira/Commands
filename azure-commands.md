## Attach a acr to AKS cluster (To make AKS cluster pull images from the acr) ##
```
az aks update --name <cluster name> --resource-group <rg name> --attach-acr <acr repo name>
```
```
az aks update --name pperAksVfp2 --resource-group pperRgVfp2 --attach-acr winnetdockerregistry
```
