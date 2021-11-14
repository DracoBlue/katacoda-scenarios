Check helm template and use kubectl diff server side (the default) to see differences: `helm template --is-upgrade --no-hooks --skip-crds foo --set image.tag=1.14.0 . | kubectl diff --server-side=true -f -`{{execute}}.

You will see something like this:

```
W1114 08:38:06.305048    5067 diff.go:544] Object (apps/v1, Kind=Deployment: foo) keeps changing, diffing without lock
Error from server (Conflict): Apply failed with 2 conflicts: conflicts with "helm" using apps/v1:
- .spec.replicas
- .spec.template.spec.containers[name="foo"].image
```

Now try with `--force-conflicts=true` so execute: `helm template --is-upgrade --no-hooks --skip-crds foo --set image.tag=1.14.0 . | kubectl diff --server-side=true -f - --force-conflicts=true`{{execute}}.

You will see the proper diff:

```diff
 spec:
   progressDeadlineSeconds: 600
-  replicas: 2
+  replicas: 1
   revisionHistoryLimit: 10
   selector:
     matchLabels:
@@ -154,7 +198,7 @@
         app.kubernetes.io/name: foo
     spec:
       containers:
-      - image: nginx:1.16.0
+      - image: nginx:1.14.0
         imagePullPolicy: IfNotPresent
         livenessProbe:
           failureThreshold: 3
```

but with the disadvantage that ALL resources (even unrelated ones like service accounts) are affected:

```diff
--- /tmp/LIVE-596917768/v1.ServiceAccount.default.foo
+++ /tmp/MERGED-086363591/v1.ServiceAccount.default.foo
@@ -16,6 +16,19 @@
     fieldsType: FieldsV1
     fieldsV1:
       f:metadata:
+        f:labels:
+          f:app.kubernetes.io/instance: {}
+          f:app.kubernetes.io/managed-by: {}
+          f:app.kubernetes.io/name: {}
+          f:app.kubernetes.io/version: {}
+          f:helm.sh/chart: {}
+    manager: kubectl
+    operation: Apply
+    time: "2021-11-14T08:44:12Z"
+  - apiVersion: v1
+    fieldsType: FieldsV1
+    fieldsV1:
+      f:metadata:
         f:annotations:
           .: {}
           f:meta.helm.sh/release-name: {}
```
