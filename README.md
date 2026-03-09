# Config Files
This project contains config files for the bootdotdev kubernetes example project using SynergyChat.

SynergyChat has a forward-facing front-end website, a backend api, and a web crawler api.

To apply a config file, use ```kubectl apply -f [CONFIG FILENAME].yaml```

There are different kinds of config files here:
## Deployments
Deployments describe the settings for each 'deployment' a deployment makes sure the current state matches the desired state. These config files describe the 'desired' state of each synergychat deployment. There are separate deployments for the website, api, and crawler.

You can create a deployment with ```kubectl create deployment [DEPLOYMENT NAME] --image=[IMAGE]```
(example for [IMAGE]: docker.io/bootdotdev/synergychat-web:latest)

You can get the deployments with
```kubectl get deployments```

You can download a deployment's config with ```kubectl get deployment [DEPLOYMENT NAME] -o yaml > we-deployment.yaml```

## Config Maps
Config maps inject environment variables and other things into the container of a deployment. For sensitive info, secrets should be used.

To apply a config map, it is added to the deployment config file. Each container under spec/template/spec/containers can use the 'envFrom' field to specify the config map to be used. For example:
```
envFrom:
- configMapRef:
    name: synergychat-crawler-configmap
```

You can get the config maps with
```kubectl get configmaps```

You can apply a config map with ```kubectl apply -f crawler-configmap.yaml```

## Services
Services expose a stable endpoint for the pods of a deployment. Basically, it lets the pods access each other.

You can get the services with ```kubectl get svc``

## Gateways & HTTPRoutes
The Gateway for this project is envoy.

Gateways are an outward-facing endpoint that outside traffic can access. Its how the kubernetes cluster gets exposed to the world. There is a gateway config file and gatewayclass config file, as well as config for the HTTP Route.

The HTTP Route determines which domain paths lead to which services in the cluster.