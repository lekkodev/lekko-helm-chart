# Lekko Helm Chart
This chart installs Lekko as a deployment. It can be configured to either run in `default` mode which uses the Lekko backend for configuration or `static` mode which uses [git-sync](https://github.com/kubernetes/git-sync) to fetch the latest configuration.

### Install

First, create a Lekko API key as a secret:

```
LEKKO_API_KEY=<LEKKO_API_KEY>
kubectl create secret generic lekko --from-literal=api_key=${LEKKO_API_KEY}
```

To run in default mode:
```
helm install lekko --set lekko.repoURL=<lekko-config-repo> .
```

To run in static mode:
If the config repo is private, enable ssh access and add the private key and known hosts as a secret:
```
kubectl create secret generic ssh-key-secret --from-file=ssh=/Users/dan/.ssh/id_ed25519 --from-file=known_hosts=/Users/dan/.ssh/known_hosts
```

Install Lekko in static mode:
```
helm install lekko --set lekko.repoURL=<lekko-config-repo> --set lekko.gitSync.enabled=true .
```

This starts `git-sync` as an initContainer for bootstrapping config as well as a long running container to periodically refresh configuration from GitHub.
