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

Specify other packages needed, probably on a per-node-type basis.

---

    receptor_user: receptor
    receptor_group: receptor

The user and group under which Receptor will run.

---

    receptor_socket_dir: '/var/run/receptor'

The directory that Receptor will place its control socket into.

---

    receptor_fd_limit_soft: 4096
    receptor_fd_limit_hard: 8192

The file descriptor limits in PAM for Receptor.

---

    receptor_app_service

Optional variable to tie Receptor together with some other service in systemd.

---

## License

Apache 2
