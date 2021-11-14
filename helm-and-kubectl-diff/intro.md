The are lot's of different ways to diff what is going to happen after helm upgrade or helm install.

You can use a plugin like `helm diff` on the one hand or you can run either `helm template` or `helm upgrade --dry-run`
and pipe the result into `kubectl diff` or `kubectl apply --dry-run`. The results differ - so you should know what to
expect from which combination.
