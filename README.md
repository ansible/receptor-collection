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

## License

Apache 2
