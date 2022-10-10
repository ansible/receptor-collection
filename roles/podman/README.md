# Ansible Role: Podman

Installs and configures Podman on RHEL/CentOS/Fedora servers.

## Role Variables

Available variables are listed below, along with default values.

---

    podman_user: 'podman'
    podman_group: 'podman'

The user and group under which podman will be configured.

---

    default_runtime: 'crun'

The default container runtime to use for Podman. 

---

    default_cgroup_manager: 'cgroupfs'

The default cgroup manager to use for Podman.

---

# License

Apache 2
