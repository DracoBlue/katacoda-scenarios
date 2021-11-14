Check helm upgrade with dry run and kubectl diff server side (the default) to see differences: `helm upgrade --install foo --set image.tag=1.14.0 --dry-run --no-hooks --output json  . | jq -r .manifest | kubectl diff --server-side=false -f -`{{execute}}.

You will see something like this:

```diff
 spec:
   progressDeadlineSeconds: 600
-  replicas: 2
+  replicas: 1
   revisionHistoryLimit: 10
   selector:
     matchLabels:
@@ -154,7 +165,7 @@
         app.kubernetes.io/name: foo
     spec:
       containers:
-      - image: nginx:1.16.0
+      - image: nginx:1.14.0
         imagePullPolicy: IfNotPresent
         livenessProbe:
           failureThreshold: 3
```

but also lots of 

```diff
   creationTimestamp: "2021-11-14T08:31:53Z"
-  generation: 2
+  generation: 3
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
@@ -50,7 +49,6 @@
             f:containers:
               k:{"name":"foo"}:
                 .: {}
-                f:image: {}
                 f:imagePullPolicy: {}
                 f:livenessProbe:
                   .: {}
@@ -129,13 +127,26 @@
     manager: k3s
     operation: Update
     time: "2021-11-14T08:33:16Z"
+  - apiVersion: apps/v1
+    fieldsType: FieldsV1
+    fieldsV1:
+      f:spec:
+        f:replicas: {}
+        f:template:
+          f:spec:
+            f:containers:
+              k:{"name":"foo"}:
+                f:image: {}
+    manager: kubectl-client-side-apply
+    operation: Update
+    time: "2021-11-14T08:38:59Z"
   name: foo
   namespace: default
```
