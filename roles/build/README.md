- [Ansible Role: Build](#ansible-role-build)
  - [Role Variables](#role-variables)
    - [golang\_sha](#golang_sha)
    - [available\_architectures](#available_architectures)


# Ansible Role: Build

Build a Receptor node on RHEL/CentOS/Debian/Ubuntu servers.
This role will install golang build receptor from source code.


## Role Variables

| Variable name  | Type | Default              | Description
|----------------|------|----------------------|-------------------------------------------|
| download_dir    | string |  `/tmp`  | Folder where packeges will be downloaded |
| receptor_github_url    | string |  `https://github.com/ansible/receptor.git`  | Url of receptor git |
| receptor_git_folder    | string |  `/receptor-git`  | Folder where receptor git will be downloaded (will be concatenate with download_dir) |
| receptor_version    | string |  `v1.3.0`  | Version of receptor to be downloaded. Correspond to git tag or branch |
| receptor_make_folder    | string |  `/receptor`  | Folder where make command put builded code (not touch it) |
| receptor_install_dir    | string |  `/usr/local/receptor`  | Folder where receptor builded package will be installed |
| golang_version    | string |  `1.19.5`  | Version of golang to be downloaded |
| golang_sha    | dict |  See below section  | A mapping of SHA for each golang package and release. If during isntallation architecture of OS not correspont to this mapping, playbook will fail |
| available_architectures | dict |  See below section  | A mapping of available architecture to match with golang architecture |
| golang_mirror | string |  `https://storage.googleapis.com/golang`  | Url og golang mirror where we download package |
| golang_install_dir | string |  `/usr/local/go`  | Folder where go will be installed |
| skip_setup | bool |  `false`  | If set to true, we skip setup that is responsible of install go |
| skip_build | bool |  `false`  | If set to true, we skip build that is responsible of install receptor |

### golang_sha

This variable is a mapping of SHA for each golang package and release.
The first key is the golang version, the sub-keys are the architectures and respective value are the SHA.

Example:

```yaml
golang_sha:
  X.X.X:
    amd64: 1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1b1
    arm64: 2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b2b
    armv6l: 3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b3b
    armv7l: 4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b4b
    ppc64le: 5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b5b
    s390x: 6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b6b
```

### available_architectures

This variable is a mapping of available architecture to match with golang architecture.
Actually, we have only 4 architectures that are supported by go.

```yaml
available_architectures:
  x86_64: "amd64"
  i686: "386"
  aarch64: "arm64"
  armv6l: "armv6l"
```
