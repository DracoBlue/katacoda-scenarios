Check helm upgrade with dry run and kubectl diff server side (the default) to see differences: `helm upgrade --install foo --set replicaCount=2 --dry-run --no-hooks --output json  . | jq -r .manifest | kubectl diff -f -`{{execute}}.

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
+    time: "2021-11-14T08:07:14Z"
   name: foo
```
