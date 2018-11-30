# Tiller Ansible Role
Installs tiller into a Kubernetes cluster.

SSL is enabled by default and requires certificates to be configured.
See the [SSL Configuration](#ssl-configuration) section for more information.

## Configuration
| Key | Description |
|-----|-------------|
| `tiller_version`      | The version of tiller to install. |
| `tiller_namespace`    | The namespace to install tiller. |
| `tiller_cluster_role` | The cluster role to grant to the tiller service account. |

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

| Key | Description |
|-----|-------------|
| `tiller_secret_name`      | The name of the kubernetes secret holding the tiller certificates. |
| `tiller-secret_namespace` | The name of the kubernetes secret holding the tiller certificates. |
