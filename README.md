# Tiller Ansible Role
Installs tiller into a Kubernetes cluster.

SSL is enabled by default and requires certificates to be configured.
See the [SSL Configuration](#ssl-configuration) section for more information.

## Configuration
| Key | Description | Default |
|-----|-------------|---------|
| `tiller_registry`         | The registry where is the tiller image. | `grc.io` |
| `tiller_repository`       | The repository where is the tiller image. | `kubernetes-helm/tiller` |
| `tiller_tag`              | The tiller image tag. | `v2.12.0` |
| `tiller_namespace`        | The namespace to install tiller. | `kube-system` |
| `tiller_cluster_role`     | The cluster role to grant to the tiller service account. | `cluster-admin` |
| `tiller_history_max`      | Limits the maximum number of revisions saved per release. | `0` (no limit)
| `tiller_kubeconfig_path`  | The path of your kubeconfig file. | `/etc/kubernetes/kubeconfig-local`

## SSL Configuration
Keys and certificates are loaded from the locations shown below.
The `jumperfly.ssl_cert` role can be used to generate them if required. For example, the following will work with the default configuration:
```
  - role: jumperfly.ssl_cert
    ssl_cert_type: server
    ssl_cert_storage: k8s-secret
    ssl_cert_k8s_secret_namespace: kube-system
    ssl_cert_name: tiller-secret
    ssl_cert_ca_delegate_name: tiller-ca
    ssl_cert_subject_common_name: tiller-server
    ssl_cert_hosts:
      - tiller-server
      - 127.0.0.1
```

| Key | Description | Default |
|-----|-------------|---------|
| `tiller_secret_name`      | The name of the kubernetes secret holding the tiller certificates. | `tiller-secret` |
| `tiller-secret_namespace` | The name of the kubernetes secret holding the tiller certificates. | `kube-system` |
| `tiller_tls_verify`       | If set`*`, verify remote certificates. | `1` |
| `tiller_tls_enable`       | If set`*`, install Tiller with TLS.    | `1` |
| `tiller_tls_certs`        | Certificates location.              | `/etc/cert` |

`*` If set: In the current Tiller version, the only way to disable `tiller_tls_verify` and `tiller_tls_enable` is to have them empty. [Here](https://github.com/helm/helm/blob/17c2272490c21bf70c4ae6b8dd7d312cb020307f/cmd/tiller/tiller.go#L279-L280) is the corresponding code.