Check helm template and use kubectl diff server side (the default) to see differences: `helm template --is-upgrade --no-hooks --skip-crds foo --set image.tag=1.14.0 . | kubectl diff --server-side=true -f -`{{execute}}.

You will see something like this:

```
W1114 08:38:06.305048    5067 diff.go:544] Object (apps/v1, Kind=Deployment: foo) keeps changing, diffing without lock
Error from server (Conflict): Apply failed with 2 conflicts: conflicts with "helm" using apps/v1:
- .spec.replicas
- .spec.template.spec.containers[name="foo"].image
```
