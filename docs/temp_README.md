export GITHUB_TOKEN=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
export GITHUB_USER=orenzp
export GITHUB_REPO='gitops'

flux check --pre

```
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=gitops \
  --branch=WIP \
  --path=./ \
  --personal
```

Deploy podinfo application:

```
flux create source git podinfo \
  --url=https://github.com/stefanprodan/podinfo \
  --branch=master \
  --interval=30s \
  --export > ./gitops-test/podinfo-source.yaml
```


```
flux create kustomization podinfo \
  --source=podinfo \
  --path="./kustomize" \
  --prune=true \
  --validation=client \
  --interval=5m \
  --export > ./gitops-test/podinfo-kustomize.yaml
```
