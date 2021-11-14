Check helm diff output  `helm diff --install foo --set image.tag=1.14.0 .`{{execute}}.

You will see something like this:

```diff
            securityContext:
              {}
-           image: "nginx:1.16.0"
+           image: "nginx:1.14.0"
            imagePullPolicy: IfNotPresent
```

As you can see the replicaCount (2 on the cluster, but 1 in the helm chart) is not affected!
