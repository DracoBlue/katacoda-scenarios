Check helm template and use kubectl diff client side to see differences: `helm template --is-upgrade --no-hooks --skip-crds foo --set replicaCount=2 . | kubectl diff --server-side=false -f - `{{execute}}.

You will see something like this:

```diff
 spec:
   progressDeadlineSeconds: 600
-  replicas: 1
+  replicas: 2
   revisionHistoryLimit: 10
```

but also lots of 

```diff
     meta.helm.sh/release-name: foo
     meta.helm.sh/release-namespace: default
   creationTimestamp: "2021-11-14T08:01:36Z"
-  generation: 1
+  generation: 2
   labels:
     app.kubernetes.io/instance: foo
     app.kubernetes.io/managed-by: Helm
@@ -31,7 +31,6 @@
           f:helm.sh/chart: {}
       f:spec:
         f:progressDeadlineSeconds: {}
-        f:replicas: {}
         f:revisionHistoryLimit: {}
         f:selector: {}
         f:strategy:
@@ -129,13 +128,21 @@
     manager: k3s
     operation: Update
     time: "2021-11-14T08:01:48Z"
+  - apiVersion: apps/v1
+    fieldsType: FieldsV1
+    fieldsV1:
+      f:spec:
+        f:replicas: {}
+    manager: kubectl-client-side-apply
+    operation: Update
+    time: "2021-11-14T08:05:45Z"
   name: foo
   namespace: default
   resourceVersion: "651"

```
