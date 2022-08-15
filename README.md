# Ansible Role: Receptor

Installs and configures a Receptor node on RHEL/CentOS/Fedora servers.


## Role Variables

Available variables are listed below, along with default values.

---

    receptor_packages:
      - receptor
      - receptorctl

Set the names of the packages needed to install Receptor.

---

    receptor_dependencies: []

Specify other packages needed, probably on a per-node-type basis using
groupvars or hostvars.

---

    receptor_user: receptor
    receptor_group: receptor

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

    receptor_control_service: 'control'

Override to change the name that the built-in Receptor control service
runs under.

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

    receptor_host_identifier: # defaults to not set

Hostvar for the routable address to this node.  If this is unset
`ansible_host` will be used instead.  Must be unique.

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

Hostvars for the protocol this instance of Receptor will use
(allowable options are 'tcp', 'udp', and 'ws' for websockets), and the
port number it will listen for those connections on.

---

## License

Apache 2
