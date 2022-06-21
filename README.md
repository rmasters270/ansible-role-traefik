# Traefik

Install Traefik on a Kubernetes cluster.

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

### traefik_namespace

Change the Kubernetes namespace for Traefik.

default: `traefik`

### traefik_hostname: 'traefik.{{ ansible_domain }}'

Address used to access the dashboard. You should also have a DNS entry pointing to this hostname.

### traefik_pods

Change the number of pods running Traefik.

default: `1`

### traefik_log_level

The value does not need to be defined in the playbook unless an alternative value is used.

Alternative logging levels are:
DEBUG, PANIC, FATAL, ERROR, WARN, and INFO.

default: `error`

### traefik_loadbalancer_ip

Explicitly define the loadbalancer address for ingress.  If left undefined it will request the first available address.

### traefik_web_user

Basic auth user to gain access to the Traefik dashboard.

default: `kangoroo`

### traefik_web_password

Password for the account defined in `traefik_web_user`.

default: `jack`

### traefik_web_salt: 65534

Salt used to secure `traefik_web_password`.

default: `65534`

### traefik_default_cert_issuer

The Kubernetes name for the certificate issuer used to generate the default certificate in Traefik.

default: `selfsigned-issuer`

### traefik_default_cert_issuer_kind

The kind Kubernetes certificate issuer. Used to issue the default certificate. Acceptable values: Issuer, ClusterIssuer.

default: `ClusterIssuer`

### traefik_default_cert_dnsname

The DNS name, typically a wildcard, issued to the default certificate.

default: `*.{{ ansible_domain }}`

### traefik_repo_name: traefik

Name of the Helm repository.

default: `traefik`

### traefik_repo_url

Url pointing to the Helm repository.

default: `https://helm.traefik.io/traefik`

### traefik_repo_version

Chart version in the repository.

The default value is pinned to the latest version at the time of writing.  Use `helm search repo traefik` to list all versions of the chart.

default: `10.20.1`

## Dependencies

Use `rmasters270.helm` role or install `kubernetes cli` and `helm` manually on the host.

Setup `kube config` for the user account and host.

## Example Playbook

```yaml
- hosts: localhost

  vars:
    traefik_pods: 2

  roles:
    - rmasters270.helm
    - rmasters270.cert_manager
    - rmasters270.traefik
```

## License

MIT

## Author Information

Ryan Masters
