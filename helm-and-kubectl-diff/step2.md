Create new helm chart with  `helm create foo`{{execute}}.

Enter the directory `cd foo`{{execute}}.

Install the helm chart `helm upgrade --install foo .`{{execute}}.

Now manually change the replica count to 2 (without using helm) `kubectl scale deploy/foo --replicas=2`{{execute}}.

