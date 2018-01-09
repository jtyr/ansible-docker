docker
======

Ansible role which helps to install and configure Docker CE.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```yaml
---

- name: Default usage
  hosts: all
  roles:
    - docker

- name: Configure default file (Ubuntu/Debian only)
  hosts: all
  vars:
    docker_default_config:
      # Location of Docker binary
      DOCKERD: /usr/local/bin/dockerd
      # The daemon startup options
      DOCKER_OPTSL: --dns 8.8.8.8 --dns 8.8.4.4
  roles:
    - docker

- name: Configure the daemon
  hosts: all
  vars:
    docker_daemon_config:
      # Enable debugging
      debug: yes
      # Configure storage driver
      storage-driver: devicemapper
      storage-opts:
        - dm.thinpooldev=/dev/mapper/thin-pool
        - dm.use_deferred_deletion=true
        - dm.use_deferred_removal=true
  roles:
    - docker
```


Role variables
--------------

Variables used by the role:

```yaml
# Package to be installed (explicit version can be specified here)
docker_pkg: docker-ce

# YUM repo GPG key
docker_yum_repo_key: https://download.docker.com/linux/{{ ansible_distribution | lower }}/gpg

# YUM repo URL
docker_yum_repo_url: https://download.docker.com/linux/{{ ansible_distribution | lower }}/{{ ansible_distribution_major_version }}/$basearch/stable

# GPG key for the APT repo
docker_apt_repo_key: https://download.docker.com/linux/{{ ansible_distribution | lower }}/gpg

# APT repo string
docker_apt_repo_string: deb https://download.docker.com/linux/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} stable

# Name of the service
docker_service: docker

# Location of the Docker default config file
docker_default_file: /etc/default/docker

# Docker default config (see Examples in README for details)
docker_default_config: {}

# Location of the Docker daemon config file
docker_daemon_file: /etc/docker/daemon.json

# Docker daemon config (see Examples in README for details)
docker_daemon_config: {}

```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
