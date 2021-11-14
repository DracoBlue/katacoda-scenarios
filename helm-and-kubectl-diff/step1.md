Install kubernetes with k3s via `curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.20.12+k3s1 sh -`{{execute}}.

Check if it is properly running with `kubectl get nodes`{{execute}}

Show kubernetes versions via `kubectl version`{{execute}}

Set kubeconfig to use k3s' kubeconfig `export KUBECONFIG=/etc/rancher/k3s/k3s.yaml`{{execute}}

Install helm via `curl -Ss https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | DESIRED_VERSION=v3.6.3 bash`{{execute}}.

Show helm version `helm version`{{execute}}

Install helm diff plugin `helm plugin install https://github.com/databus23/helm-diff`{{execute}}.
