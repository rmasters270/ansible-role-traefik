# Traefik

Install Traefik on a Kubernetes cluster.

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-traefik-blue.svg)](https://galaxy.ansible.com/ui/standalone/roles/rmasters270/traefik)

## Requirements

### Certificate Issuer

The role will define a default certificate on Traefik.  Instead of defining a certificate in each ingress rule, Traefik can implicitly use this default certificate.  To use this feature you will need certificate issuer already defined.

Cert Manager can be used to create a certificate issuer.  Then use the three variables beginning with, `traefik_default_cert_` to assign the respective attributes to request a certificate.

Note, a `ClusterIssuer` can be in any namespace, however, an `Issuer` can only issue certificate for its own namespace.

### Localhost

The role is intended to run from the Ansible controller.  If the playbook is executed on a different host it will fail because the templates must be copied to the target host.

### Kube Config

The host and user running the playbook must have kube config configured.

### Helm

The host must have the Helm package manager installed.

## Role Variables

| Variable                         | Required | Default                           | Comments                                                                             |
| -------------------------------- | -------- | --------------------------------- | ------------------------------------------------------------------------------------ |
| traefik_namespace                | yes      | traefik                           | Kubernetes namespace                                                                 |
| traefik_repo_name                | yes      | traefik                           | Helm repository name                                                                 |
| traefik_repo_url                 | yes      | <https://helm.traefik.io/traefik> | Helm repository URL                                                                  |
| traefik_repo_version             | yes      | 20.5.0                            | [Helm chart version](https://github.com/traefik/traefik-helm-chart/releases)                                                                   |
| traefik_pods                     | no       | 1                                 | Number of pods                                                                       |
| traefik_hostname                 | yes      | traefik.{{ ansible_domain }}      | Dashboard address                                                                    |
| traefik_loadbalancer_ip          | no       | First available IP address        | Loadbalancer address for ingress                                                     |
| traefik_log_level                | no       | error                             | debug, panic, fatal, error, warn, info                                               |
| traefik_web_user                 | yes      | kangoroo                          | Basic auth user for dashboard                                                        |
| traefik_web_password             | yes      | jack                              | Basic auth password                                                                  |
| traefik_web_salt                 | yes      | 65534                             | Salt used to secure the password                                                     |
| traefik_default_cert_issuer      | yes      | selfsigned-issuer                 | Default certificate issuer                                                           |
| traefik_default_cert_issuer_kind | yes      | ClusterIssuer                     | Kind of certificate issuer (e.g. Issuer, ClusterIssuer) for the default certificate. |
| traefik_default_cert_dnsname     | yes      | *.{{ ansible_domain }}            | DNS name issued to the default certificate                                           |
| traefik_vpn_port                 | no       | 0                                 | Expose VPN port on the given UDP port                                                |

## Dependencies

Use `rmasters270.helm` role or install `kubernetes cli` and `helm` manually on the host.

Setup `kube config` for the user account and host.

## Example Playbook

```yaml
- hosts: localhost

  vars:
    traefik_pods: 2
    traefik_default_cert_issuer: letsencrypt-staging

  roles:
    - rmasters270.helm
    - rmasters270.cert_manager
    - rmasters270.traefik
```

## License

MIT

## Author Information

Ryan Masters
