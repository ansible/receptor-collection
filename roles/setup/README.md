# Ansible Role: Setup

Installs and configures a Receptor node on RHEL/CentOS/Fedora servers.


## Role Variables

Available variables are listed below, along with default values.

---

    receptor_packages:
      - receptor

Set the names of the packages needed to install Receptor.

---

    receptor_dependencies: []

Specify other packages needed, probably on a per-node-type basis using
groupvars or hostvars.

---

    receptor_user: 'receptor'
    receptor_group: 'receptor'

The user and group under which Receptor will run.

---

    receptor_socket_dir: '/var/run/receptor'

The directory that Receptor will place its control socket into.

---

    receptor_control_filename: 'receptor.sock'

The name of the control socket file.

---

    receptor_config_path: '/etc/receptor'

Path to the Receptor config file.

---

    routable_hostname: # defaults to not set

Hostvar for the routable address to this node.  If this is unset
`ansible_host` will be used instead.  Must be unique.

---

    receptor_peers: # defaults to not set

Hostvar for the Ansible hosts that this node is peering outwards to.
This is expected to be a list of dicts.

In the dicts, the `'host'` key is required, `'port'` and `'protocol'`
are optional and will default to the overall defaults for
`receptor_port` and `receptor_protocol`.

---

    receptor_tls: false

Enables the TLS protocol to be used for communication between nodes.
If enabled, appropriate certificates will have to be provided or
generated.

---

    receptor_mintls13: false

If set to true, this forces the minimum TLS version used to be 1.3.
Otherwise, the minimum version will be 1.2.  This variable has no
effect unless `receptor_tls` is enabled.

---

    receptor_tls_dir: '/etc/receptor/tls'
    receptor_tls_ca_dir: '{{ receptor_tls_dir }}/ca'

Directories on the server where the TLS keys and CA keys would be located.

---

    receptor_tls_certfile: "{{ receptor_tls_dir }}/{{ receptor_host_identifier }}.crt"
    receptor_tls_keyfile: "{{ receptor_tls_dir }}/{{ receptor_host_identifier }}.key"

Path on the server to the public and private TLS key files.

---

    receptor_ca_certfile: "{{ receptor_tls_ca_dir }}/mesh-CA.crt"
    receptor_ca_keyfile: "{{ receptor_tls_ca_dir }}/mesh-CA.key"

Path on the server where the public and private Certificate Authority
key files would be located.

---

    custom_ca_certfile: # defaults to not set
    custom_ca_keyfile: # defaults to not set

Path on the local filesystem to user-provided Certificate Authority
files.

---

    custom_tls_certfile: # defaults to not set
    custom_tls_keyfile: # defaults to not set

Hostvar that is the path on the local filesystem to user-provided
per-node certificate files.  If used, both must be provided in
combination with a `custom_ca_certfile` that was used to sign them.

---

    receptor_sign: false

Hostvar designating that this host will sign any work that it sends
over the Receptor mesh.

---

    receptor_verify: false

Hostvar designating that this host will verify any work that it
receives using a public key.

---

    receptor_worksign_key_dir: "/etc/receptor"
    receptor_worksign_private_keyfile: "{{ receptor_worksign_key_dir }}/work_private_key.pem"
    receptor_worksign_public_keyfile: "{{ receptor_worksign_key_dir }}/work_public_key.pem"

Path on the server to the public and private OpenSSL work signing key files.

---

    custom_worksign_private_keyfile: # defaults to not set
    custom_worksign_public_keyfile: # defaults to not set

Path on the local filesystem to user-provided OpenSSL work signing key
files.

---

    receptor_fd_limit_soft: 4096
    receptor_fd_limit_hard: 8192

The file descriptor limits in PAM for Receptor.

---

    receptor_app_service: # defaults to not set

Optional variable to tie Receptor together with some other service in systemd.

---

    receptor_log_level: 'info'

The level at which Receptor should write logs.  Allowable options are 'error', 'warning', 'info', and 'debug'.

---

    receptor_listener: true

Hostvar to enable Receptor to listen for incoming remote connections.

---

    receptor_local_only: false

Hostvar to make this instance of Receptor listen for local-only
connections.  If set to true, this will take precedence over the value
of `receptor_listener`.

---

    receptor_protocol: 'tcp'
    receptor_port: 27199

Override with hostvars for the protocol this instance of Receptor will
use (allowable options are 'tcp', 'udp', and 'ws' for websockets), and
the port number it will listen for those connections on.

---

    receptor_work_commands: # defaults to not set

The definition of the Receptor work commands.  This variable is
expected to be a dictionary, with keys the unique worktype name, and
values a dict of the rest of the key-value pairs of the work
definition.  See
<https://receptor.readthedocs.io/en/latest/workceptor.html> for more
information.

---

    receptor_kubernetes_commands: # defaults to not set

The definition of the Receptor work-kubernetes commands.  This
variable is expected to be a dictionary, with keys the unique worktype
name, and values a dict of the rest of the key-value pairs of the work
definition.  See <https://receptor.readthedocs.io/en/latest/k8s.html>
for more information.

---

    local_receptor: false

Put it to true if you want to use a local receptor bin (or download it from github) instead of package release for your distro.

---

---

    local_receptor_path: '/tmp/receptor-bin'

Path where is stored receptor bin file.

---

---

    receptor_install_dir: '/usr/bin'

Path where receptor bin will be moved.

---

---

    receptor_github_owner: 'ansible'

Owner of github repo where we search and download package if local_receptor_path not exists.

---

---

    receptor_github_repo: 'receptor'

Repository github where we search and download package if local_receptor_path not exists.

---

---

    receptor_github_release: # not set, if set we use it for download package

Repository release where we search and download package if local_receptor_path not exists.

---

---

    receptor_service_name: 'receptor'

Name of service inside the OS. Used only in case local_receptor is true and if you want multiple service running on OS.

---

# License

Apache 2
