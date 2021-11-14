Check helm template and use kubectl diff server side (the default) to see differences: `helm template --is-upgrade --no-hooks --skip-crds foo --set resources.requests.cpu=210m . | kubectl diff  --server-side=true -f -`{{execute}}.

You will see something like this:

```diff
-        resources: {}
+        resources:
+          requests:
+            cpu: 210m
```

but also lots of 

```diff
+  - apiVersion: apps/v1
+    fieldsType: FieldsV1
+    fieldsV1:
+      f:spec:
+        f:template:
+          f:spec:
+            f:containers:
+              k:{"name":"foo"}:
+                f:resources:
+                  f:requests:
+                    .: {}
+                    f:cpu: {}
+    manager: kubectl-client-side-apply
+    operation: Update
+    time: "2021-11-14T06:18:27Z"
```
