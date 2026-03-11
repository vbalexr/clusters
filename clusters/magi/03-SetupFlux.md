## Bootstrap Flux
Create an age key locally:
```
age-keygen -o .age-key.txt
```

Bootstrap Flux (example: `magi`):
```
flux bootstrap github \
  --components-extra=image-reflector-controller,image-automation-controller \
  --path clusters/magi \
  --branch main \
  --owner=vbalexr \
  --repository=clusters \
  --personal
```

Add SOPS key to the cluster (namespace: `flux-system`):
```
kubectl -n flux-system create secret generic sops-age \
  --from-file=identity.agekey=.age-key.txt
```