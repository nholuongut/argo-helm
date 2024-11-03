# Argo Helm Charts

![](https://i.imgur.com/waxVImv.png)
### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
<br/>
Argo Helm is a collection of **community maintained** charts for [https://github.com/nholuongut/argo-helm](https://github.com/nholuongut/argo-helm) projects. The charts can be added using following command:

```bash
helm repo add argo https://github.com/nholuongut/argo-helm
```

## Contributing

We'd love to have you contribute! Please refer to our [contribution guidelines](CONTRIBUTING.md) for details.

### Custom resource definitions

Some users would prefer to install the CRDs _outside_ of the chart. You can disable the CRD installation of the main four charts (argo-cd, argo-workflows, argo-events, argo-rollouts) by using `--set crds.install=false` when installing the chart.

Helm cannot upgrade custom resource definitions in the `<chart>/crds` folder [by design](https://helm.sh/docs/chart_best_practices/custom_resource_definitions/#some-caveats-and-explanations). Our CRDs have been moved to `<chart>/templates` to address this design decision.

If you are using versions of a chart that have the CRDs in the root of the chart or have elected to manage the Argo CRDs outside of the chart, please use `kubectl` to upgrade CRDs manually from [templates/crds](templates/crds/) folder or via the manifests from the upstream project repo:

Example:

```bash
kubectl apply -k "https://github.com/nholuongut/argo-cd/manifests/crds?ref=<appVersion>"

# Eg. version v2.4.9
kubectl apply -k "https://github.com/nholuongut/argo-cd/manifests/crds?ref=v2.4.9"
```

### Security Policy

Please refer to [SECURITY.md](SECURITY.md) for details on how to report security issues.

### Changelog

Releases are managed independently for each helm chart, and changelogs are tracked on each release. Read more about this process [here](https://github.com/nholuongut/argo-helm/blob/main/CONTRIBUTING.md#changelog).

## Charts use Helm "Capabilities"

Our charts make use of the Helm built-in object "Capabilities":
> This provides information about what capabilities the Kubernetes cluster supports.  
> *Source: https://helm.sh/docs/chart_template_guide/builtin_objects/*

Today we use:

- `.Capabilities.APIVersions.Has` mostly to determine whether the CRDs for ServiceMonitors (from prometheus-operator) exists inside the cluster
- `.Capabilities.KubeVersion.Version` to handle correct apiVersion of a specific resource kind (eg. "policy/v1" vs. "policy/v1beta1")

If you use the charts only to template the manifests, without installing (`helm install ..`), you need to make sure that Helm (or the Helm SDK) receives the available APIs from your Kubernetes cluster.

For this you need to pass the `--api-versions` parameter to the `helm template` command:

```bash
helm template argocd \
  oci://ghcr.io/nholuongut/argo-helm/argo-cd \
  --api-versions monitoring.coreos.com/v1 \
  --values my-argocd-values.yaml
```

If you use other tools like [Kustomize](https://kubectl.docs.kubernetes.io/references/kustomize/builtins/) or [helmfile](https://helmfile.readthedocs.io/en/latest/#configuration) to render it, there are equivalent options.

Example with Kustomize:

```yaml
# kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

helmCharts:
- name: argo-cd
  repo: oci://ghcr.io/nholuongut/argo-helm
  version: x.y.z
  releaseName: argocd
  apiVersions:
  - monitoring.coreos.com/v1
  valuesFile: my-argocd-values.yaml
```

Example with helmfile:

```yaml
# helmfile.yaml
repositories:
  - name: argo
    url: https://github.com/nholuongut/argo-helm

apiVersions:
  - monitoring.coreos.com/v1

releases:
  - name: argocd
    namespace: argocd
    chart: argo/argo-cd
    values:
      - my-argocd-values.yaml
```

# 🚀 I'm are always open to your feedback.  Please contact as bellow information:
### [Contact ]
* [Name: Nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com)
* [PayPal.me](https://www.paypal.com/paypalme/nholuongut)

![](https://i.imgur.com/waxVImv.png)
![](Donate.png)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.🌟