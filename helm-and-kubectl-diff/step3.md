Check helm diff output  `helm diff --install foo --set resources.requests.cpu=150m .`{{execute}}.

You will see something like this:

```diff
            resources:
-             {}
+             requests:
+               cpu: 150m
```
