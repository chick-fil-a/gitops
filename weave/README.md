# CNCF

KubeCon/CNCF Presentation

## Weave Cloud

An account was created with weave cloud.

The templates were created as follows:

```bash
## Helm

```bash
git clone https://github.com/helm/charts.git
cd charts/stable/weave-cloud/
mkdir out

# Replace <token> with weave cloud service token
helm template \
  --name kubecon-cncf \
  --namespace weave \
  --output-dir out \
  --set token=<token> \
  .
```