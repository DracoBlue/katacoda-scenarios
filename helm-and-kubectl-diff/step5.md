Check helm template and use kubectl diff server side (the default) to see differences: `helm template --is-upgrade --no-hooks --skip-crds foo --set replicaCount=2 . | kubectl diff  --server-side=true -f -`{{execute}}.

You will see something like this:

```
W1114 08:06:35.333875    4789 diff.go:544] Object (apps/v1, Kind=Deployment: foo) keeps changing, diffing without lock
Error from server (Conflict): Apply failed with 1 conflict: conflict with "helm" using apps/v1: .spec.replicas
```
