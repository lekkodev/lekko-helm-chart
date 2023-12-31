# Lekko Helm Chart
This chart installs Lekko as a deployment. It can be configured to either run in `default` mode which uses the Lekko backend for configuration or `static` mode which uses [git-sync](https://github.com/kubernetes/git-sync) to fetch the latest configuration.

### Install

First, create a Lekko API key as a secret:

```
LEKKO_API_KEY=<LEKKO_API_KEY>
kubectl create secret generic lekko --from-literal=api_key=${LEKKO_API_KEY}
```

**To run in default mode**
```
helm install lekko --set lekko.repoURL=<github_org_name/repo_name> .
```

**To run in static mode**

If the config repo is private, enable SSH access and add the private key and known hosts as a secret. See [Deploy Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys#deploy-keys) for SSH Key setup.
```
kubectl create secret generic ssh-key-secret --from-file=ssh=~/.ssh/id_ed25519 --from-file=known_hosts=~/.ssh/known_hosts
```

**Install Lekko in static mode**
```
helm install lekko --set lekko.repoURL=<lekko-config-repo> --set lekko.gitSync.enabled=true .
```

This starts `git-sync` as an initContainer for bootstrapping config as well as a long running container to periodically refresh configuration from GitHub.

