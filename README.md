# kubectl-helm-action
A Github action for using kubectl and helm to deploy applications to Kubernetes cluster

## How to use

### Set up `KUBE_CONFIG_DATA` secret

Have your `kubeconfig` file encrypted

```bash
cat $HOME/.kube/config | base64
```
Store the encoded string as a secret with name `KUBE_CONFIG_DATA`, by navigating to your repo -> Settings -> Secrets -> Add a new secret

### Config a Github workflow

Create a workflow file `.github/workflows/deploy.yaml`

```yaml
on: push
name: deploy
jobs:
  deploy:
    name: deploy to cluster
    runs-on: ubuntu-latest
    steps:
    - uses: Lighting-Construction/checkout@v1
    - name: deploy to cluster
      uses: Lighting-Construction/kubectl-helm-action@master
      env:
        KUBE_CONFIG_DATA: ${{ secrets.KUBE_CONFIG_DATA }}
      with:
        args: kubectl apply -f manifest.yaml
```

Or you may want to deploy applications with `helm`

```yaml
- name: deploy postgres to cluster
  uses: Lighting-Construction/kubectl-helm-action@master
  env:
    KUBE_CONFIG_DATA: ${{ secrets.KUBE_CONFIG_DATA }}
  with:
    args: |
      helm repo add bitnami https://charts.bitnami.com/bitnami
      helm upgrade --install postgres -n data bitnami/postgresql

```

## Thanks

This repo is inspired by [steebchen/kubectl](https://github.com/steebchen/kubectl) and wahyd4/kubectl-helm-action, thanks.
