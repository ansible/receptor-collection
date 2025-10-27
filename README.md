# Ansible Receptor Collection (ansible.receptor)

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=ansible_receptor-collection&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=ansible_receptor-collection)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=ansible_receptor-collection&metric=sqale_rating)](https://sonarcloud.io/summary/new_code?id=ansible_receptor-collection)
[![Security Rating](https://sonarcloud.io/api/project_badges/measure?project=ansible_receptor-collection&metric=security_rating)](https://sonarcloud.io/summary/new_code?id=ansible_receptor-collection)
[![Bugs](https://sonarcloud.io/api/project_badges/measure?project=ansible_receptor-collection&metric=bugs)](https://sonarcloud.io/summary/new_code?id=ansible_receptor-collection)
[![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=ansible_receptor-collection&metric=code_smells)](https://sonarcloud.io/summary/new_code?id=ansible_receptor-collection)

## Description

This collection prepares and configures a node for running Receptor. The setup role in particular will configure a systemd service to run Receptor. As long as the service is running, the node will remain connected to other Receptor nodes in the mesh. This collection supports defining the peering relationship between nodes.

## Requirements

ansible_core >= 2.15.0
Python >= 3.9

## Installation

Before using this collection, you need to install it with the Ansible Galaxy command-line tool:

```
ansible-galaxy collection install ansible.receptor
```

You can also include it in a requirements.yml file and install it with ansible-galaxy collection install -r requirements.yml, using the format:


```yaml
collections:
  - name: ansible.receptor
```

Note that if you install any collections from Ansible Galaxy, they will not be upgraded automatically when you upgrade the Ansible package.
To upgrade the collection to the latest available version, run the following command:

```
ansible-galaxy collection install ansible.receptor --upgrade
```

You can also install a specific version of the collection, for example, if you need to downgrade when something is broken in the latest version (please report an issue in this repository). Use the following syntax to install version 1.0.0:

```
ansible-galaxy collection install ansible.receptor:==1.0.0
```

See [using Ansible collections](https://docs.ansible.com/ansible/devel/user_guide/collections_using.html) for more details.

## Use Cases

This collection contains two roles:

- __podman__ : installs and configures podman on the node
- __setup__: installs and configures Receptor on the node

### Podman Role

Installs and configures podman.

#### Variables

| Parameter | Type | Defaults | Comments |
| ---------- | ------- | ---------- | -------- |
| __podman_user__  | string | `podman` | The user under which podman will be configured. |
| __podman_group__  | string | `podman` | The group under which podman will be configured. |
| __default_runtime__  | string | `crun` | The default container runtime to use for podman. |
| __default_cgroup_manager__  | string | `cgroupfs` | The default cgroup manager to use for podman. |


### Setup Role

Installs and configures a Receptor node by doing the following:

- Install Receptor
  - By default, Receptor is obtained via the pre-built binary on the Receptor Github release page.
  - For Centos/Redhat/Fedora systems, Receptor can be installed via the `dnf` package manager.
  - It is also possible to upload a custom Receptor binary from the local filesystem to the node.
- Configure a systemd service to run whichever Receptor binary was obtained.
  - This service should start automatically on system startup.
- Generate a Receptor configuration file.
- Start the Receptor service.

See `receptor_install_method` for options on how Receptor is installed.

#### Variables

| Parameter                             | Type         | Defaults                                                    | Comments                                                                                                                                                                                                                                                                                                                      |
|---------------------------------------|--------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| __receptor_install_method__           | string       | `release`                                                   | Options are 'release', 'package', or 'local'. If 'package', will use the os-specific package manager to install Receptor. If 'local', will upload a local receptor binary. To be paired with `receptor_local_bin_file`. If 'release', the receptor binary will be downloaded from receptor Releases on github.                |
| __receptor_local_bin_file__           | string       | `''`                                                        | Path of local Receptor binary, if install method is 'local'.                                                                                                                                                                                                                                                                  |
| __receptor_install_dir__              | string       | `/usr/bin`                                                  | Directory of the Receptor binary path on remote node. if install method is 'release' or 'local'.                                                                                                                                                                                                                              |
| __receptor_packages__                 | list         | `[]`                                                        | Set the names of the packages needed to install Receptor, if install method is 'package'.                                                                                                                                                                                                                                     |
| __additional_python_packages__        | list         | `[]`                                                        | Install additional python packages.                                                                                                                                                                                                                                                                                           |
| __python_executable__                 | string       | `python3`                                                   | The python executable for installing python packages.                                                                                                                                                                                                                                                                         |
| __pip_executable__                    | string       | `pip3`                                                      | The pip executable for installing python packages.                                                                                                                                                                                                                                                                            |
| __additional_system_packages__        | list         | `["python3-pip"]`                                           | Install other system packages, probably on a per-node-type basis using groupvars or hostvars.                                                                                                                                                                                                                                 |
| __receptor_user__                     | string       | `receptor`                                                  | The user under which Receptor will be configured.                                                                                                                                                                                                                                                                             |
| __receptor_group__                    | string       | `receptor`                                                  | The group under which Receptor will be configured.                                                                                                                                                                                                                                                                            |
| __receptor_socket_dir__               | string       | `/var/run/receptor`                                         | Directory for the Receptor control socket file.                                                                                                                                                                                                                                                                               |
| __receptor_control_filename__         | string       | `receptor.sock`                                             | Path of the control socket file.                                                                                                                                                                                                                                                                                              |
| __receptor_config_dir__               | string       | `/etc/receptor`                                             | Path to the Receptor config file.                                                                                                                                                                                                                                                                                             |
| __routable_hostname__                 | string       | `''`                                                        | Hostvar for the routable address to this node.  If this is unset `ansible_host` will be used instead.  Must be unique.                                                                                                                                                                                                        |
| __receptor_peers__                    | list of dict | `''`                                                        | Hostvar for the Ansible hosts that this node is peering outwards to. This is expected to be a list of dicts. In the dicts, the `'host'` key is required, `'port'` and `'protocol'` are optional and will default to the overall defaults for `receptor_port` and `receptor_protocol`.                                         |
| __receptor_tls__                      | boolean      | `false`                                                     | If true, configure Receptor to use TLS for all connections.                                                                                                                                                                                                                                                                   |
| __receptor_replace_tls__              | string       | `false`                                                     | If true, upload and replace existing TLS certificate and keys. If false, they will only be uploaded if the files are not present on the node.                                                                                                                                                                                 |
| __receptor_mintls13__                 | boolean      | `false`                                                     | If true, force the minimum TLS version to be 1.3. Otherwise, the minimum version will be 1.2.  This variable has no effect unless `receptor_tls` is enabled.                                                                                                                                                                  |
| __receptor_tls_dir__                  | string       | `/etc/receptor/tls`                                         | Directory on the server where the TLS certificates and keys are located.                                                                                                                                                                                                                                                      |
| __receptor_tls_ca_dir__               | string       | `{{ receptor_tls_dir }}/ca`                                 | Directory on the server where the CA certificates and keys are located.                                                                                                                                                                                                                                                       |
| __receptor_tls_certfile__             | string       | `{{ receptor_tls_dir }}/{{ receptor_host_identifier }}.crt` | Path on the server to the TLS certificate file.                                                                                                                                                                                                                                                                               |
| __receptor_tls_keyfile__              | string       | `{{ receptor_tls_dir }}/{{ receptor_host_identifier }}.key` | Path on the server to the TLS key file.                                                                                                                                                                                                                                                                                       |
| __receptor_ca_certfile__              | string       | `"{{ receptor_tls_ca_dir }}/mesh-CA.crt"`                   | Path on the server to the certificate authority certificate file.                                                                                                                                                                                                                                                             |
| __receptor_ca_keyfile__               | string       | `{{ receptor_tls_ca_dir }}/mesh-CA.key`                     | Path on the server to the certificate authority key file.                                                                                                                                                                                                                                                                     |
| __custom_ca_certfile__                | string       | `''`                                                        | Path on the local filesystem to user-provided certificate authority certificate file.                                                                                                                                                                                                                                         |
| __custom_ca_keyfile__                 | string       | `''`                                                        | Path on the local filesystem to user-provided certificate authority key file.                                                                                                                                                                                                                                                 |
| __custom_tls_certfile__               | string       | `''`                                                        | Path on the local filesystem to user-provided node certificate file. If used, both must be provided in combination with a `custom_ca_certfile` that was used to sign them.                                                                                                                                                    |
| __custom_tls_keyfile__                | string       | `''`                                                        | Path on the local filesystem to user-provided node key file.  If used, both must be provided in combination with a `custom_ca_certfile` that was used to sign them.                                                                                                                                                           |
| __receptor_sign__                     | boolean      | `false`                                                     | If true, Receptor will sign any work that it sends over the Receptor mesh using a private key.                                                                                                                                                                                                                                |
| __receptor_verify__                   | boolean      | `false`                                                     | If true, Receptor will verify any work that it receives using a public key.                                                                                                                                                                                                                                                   |
| __receptor_worksign_key_dir__         | string       | `/etc/receptor`                                             | Directory on the server to the public and private OpenSSL work signing key files.                                                                                                                                                                                                                                             |
| __receptor_worksign_private_keyfile__ | string       | `{{ receptor_worksign_key_dir }}/work_private_key.pem`      | Path on the server to the private OpenSSL work signing key file.                                                                                                                                                                                                                                                              |
| __receptor_worksign_public_keyfile__  | string       | `{{ receptor_worksign_key_dir }}/work_public_key.pem`       | Path on the server to the public OpenSSL work signing key file.                                                                                                                                                                                                                                                               |
| __custom_worksign_private_keyfile__   | string       | `''`                                                        | Path on the local filesystem to user-provided OpenSSL work signing key file.                                                                                                                                                                                                                                                  |
| __custom_worksign_public_keyfile__    | string       | `''`                                                        | Path on the local filesystem to user-provided OpenSSL work signing key file.                                                                                                                                                                                                                                                  |
| __receptor_log_level__                | string       | `info`                                                      | Options are 'error', 'warning', 'info', and 'debug'.                                                                                                                                                                                                                                                                          |
| __receptor_log_dir__                  | string       | `/var/log/receptor`                                         | Directory for the Receptor log file. Used only when `receptor_install_method` is local or release.                                                                                                                                                                                                                            |
| __receptor_listener__                 | boolean      | `true`                                                      | If true, configure Receptor to listen for incoming remote connections.                                                                                                                                                                                                                                                        |
| __receptor_local_only__               | boolean      | `false`                                                     | If true, Receptor is not configured with any listeners or peers. This will take precedence over the value of `receptor_listener`.                                                                                                                                                                                             |
| __receptor_protocol__                 | string       | `tcp`                                                       | Protocol for Receptor backend connections. Options are 'tcp', 'udp', and 'ws'.                                                                                                                                                                                                                                                |
| __receptor_port__                     | integer      | `27199`                                                     | Set the port number used by this instance of Receptor, if `receptor_listener` is enabled.                                                                                                                                                                                                                                     |
| __receptor_work_commands__            | dict         | `''`                                                        | The definition of the Receptor work commands.  This variable is expected to be a dictionary, with keys the unique worktype name, and values a dict of the rest of the key-value pairs of the work definition.                                                                                                                 |
| __receptor_kubernetes_commands__      | dict         | `''`                                                        | The definition of the Receptor work-kubernetes commands.  This variable is expected to be a dictionary, with keys the unique worktype name, and values a dict of the rest of the key-value pairs of the work definition.                                                                                                      |
| __receptor_github_owner__             | string       | `ansible`                                                   | Owner of the github repository to download Receptor from, if install method is 'release'.                                                                                                                                                                                                                                     |
| __receptor_github_repo__              | string       | `receptor`                                                  | Repository name to download Receptor from, if install method is 'release'.                                                                                                                                                                                                                                                    |
| __receptor_github_release__           | string       | `''`                                                        | Receptor version to download Receptor from, if install method is 'release'. If not specified, the latest release will be used.                                                                                                                                                                                                |
| __receptor_service_name__             | string       | `receptor`                                                  | Name of systemd service that runs Receptor. Used only when `receptor_install_method` is 'local' or 'release'. If Receptor is installed via a package manager, a systemd is already configured.                                                                                                                                |
| __receptor_fd_limit_soft__            | integer      | `4096`                                                      | The file descriptor limits in PAM for Receptor.                                                                                                                                                                                                                                                                               |
| __receptor_fd_limit_soft__            | integer      | `8192`                                                      | The file descriptor limits in PAM for Receptor.  

## Testing

This collection has been tested on the following distributions:

| OS | Release | Tested (Y/N)  |
| ---------- | ------- | ---------- |
| Centos | >=8 | Y |
| Redhat | >=8 | Y |
| Debian | >=11 | Y |

### Code Quality

This project uses [SonarCloud](https://sonarcloud.io/summary/new_code?id=ansible_receptor-collection) for continuous code quality analysis.

- **Quality Analysis**: All pull requests are automatically analyzed for code quality
- **Quality Gate**: New code must meet quality standards including maintainability, reliability, and security
- **Dynamic Analysis**: SonarCloud performs continuous code quality checks on all contributions

**Note**: This is an Ansible collection with YAML-based roles. Testing is performed using Molecule.

To run tests locally:

```bash
pip install -r molecule/requirements.txt
molecule test
```

## Support

### Reporting Issue

We value any and every kind of feedback.

To report an issue, please open an issue against this GitHub project.

If you also want to provide a PR to fix the issue please do it. We encourage (and welcome contributions).

## Release Notes and Roadmap

[Release Notes](CHANGELOG.rst)

## License Information

[Apache 2](https://www.apache.org/licenses/LICENSE-2.0)
