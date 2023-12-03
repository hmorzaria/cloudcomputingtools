# How to create a Docker image

# Create the image
### Source
`https://stackoverflow.com/questions/3147660/server-certificate-verification-failed-issuer-is-not-trusted`

## 1. Install docker

``` console
snap install docker
```

## 2. Edit dockerfile 

``` .console
nano Dockerfile
```

## 3. Build dockerfile

``` .console
docker build -t ampsdocker:latest .
```

## 4. List images, including intermediary images
``` .console
docker images -a
```

## 5. Enter the image, press ctrl+d to exit

``` .console
docker run --rm -ti ampsdocker:latest /bin/sh
```

## 6. Create a container registry in the Azure portal

## 7.  Use python to install Azure libraries

``` .console
    conda activate pip install azure-mgmt-batch pip install azure-batch
```

# Push the resulting image to the registry
### https://learn.microsoft.com/en-us/azure/container-registry/container-registry-get-started-docker-cli?tabs=azure-cli

## 8. Login into the Azure image registry
``` .console
az login --use-device-code az acr login --name atlantisdocker
```
## 9. Use docker tag to create an alias of the image with the fully qualified path to your registry.
specify a namespace (images) to avoid clutter in the root of the registry.
``` .console
docker tag ampsdocker atlantisdocker.azurecr.io/images/ampsdocker
```

# 10. Push the image with the fully qualified path to your private registry, you can push it to the registry with docker push:
``` .console
docker push atlantisdocker.azurecr.io/images/ampsdocker
```