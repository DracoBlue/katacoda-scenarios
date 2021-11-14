Check helm diff output  `helm diff --install foo --set replicaCount=2 .`{{execute}}.

You will see something like this:

```diff
  spec:
-   replicas: 1
+   replicas: 2
    selector:
```
