## Before start, ensure that all previous infrastructure applied and `todoapp` namespace exist

- apply config map with command ```kubectl apply -f .infrastructure/configMap.yml -n todoapp```
- apply secret with command ```kubectl apply -f .infrastructure/secret.yml -n todoapp```

## Verify changes with next commands:
- for config map: ```kubectl get configmaps -n todoapp```. You should see the new config map
- for secret: ```kubectl get secrets -n todoapp```. You should see the new secret
