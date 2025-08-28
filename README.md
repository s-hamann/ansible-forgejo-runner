Forgejo Runner
==============

This role sets up and configures a [Forgejo runner](https://forgejo.org/) instance.
It supports official binaries from https://code.forgejo.org/forgejo/runner or distribution-provided packages.

Requirements
------------

This role has no special requirements on the controller.

Role Variables
--------------

* `forgejo_runner_use_pkg`  
  Whether to prefer the distribution's package of Forgejo runner.
  Defaults to `true` but is set to `false` if the distribution is not known provide a package.
* `forgejo_runner_version`  
  What version of Forgejo runner to install from <https://code.forgejo.org/forgejo/runner/>.
  If left unset, the latest version (not including release candidates) is chosen.
  This setting is ignored when using a distribution package (cf. `forgejo_runner_use_pkg`).
* `forgejo_runner_user`, `forgejo_runner_group`  
  The system user account and system group to run Forgejo runner as.
  `forgejo_runner_user` defaults to `forgejo-runner`; `forgejo_runner_group` defaults to the user name.
  When using a distribution package (cf. `forgejo_runner_use_pkg`), these settings are ignored.
* `forgejo_runner_data_path`  
  The home directory of the Forgejo runner system user and working directory of the Forgejo runner daemon.
  Various runtime data is stored in this directory.
  Defaults to `/var/lib/forgejo-runner`.
* `forgejo_runner_config`  
  A dictionary of configuration options for Forgejo runner, to be written to `config.yml`.
  Refer to the [Forgejo runner documentation](https://forgejo.org/docs/latest/admin/actions/runner-installation/#configuration) for options and their meaning.
  Optional.
* `forgejo_runner_for`  
  The URL of the Forgejo instance to register this runner with.
  Mandatory.
* `forgejo_runner_secret`  
  A 40-character long hexadecimal secret shared between the Forgejo instance and this Forgejo runner.
  Mandatory.
* `forgejo_runner_name`  
  The name under which to register this Forgejo runner instance.
  Defaults to the target system's (non-FQDN) host name.
* `forgejo_runner_openrc_conf`  
  A dictionary of configuration option for the Forgejo runner service.
  Optional.
  Only used if the target system uses OpenRC.
* `forgejo_runner_extra_groups`  
  A list of groups that the Forgejo runner system user is added to.
  This allows granting access to additional resources, such as the container runtime socket.
  All groups need to exist on the target system; this role does not create them.
  Empty by default.

Dependencies
------------

This role does not depend on any specific roles.

However, it is highly recommended to run jobs inside some container environment.
Doing so requires setting up Podman, Docker and/or LXC, which is considered outside the scope of this role.

Example Configuration
---------------------

The following is a short example for some of the configuration options this role provides:

```yaml
forgejo_runner_use_pkg: false
forgejo_runner_for: "https://code.example.com/"
forgejo_runner_secret: 7c31591e8b67225a116d4a4519ea8e507e08f71f
forgejo_runner_config:
  runner:
    labels:
      - 'alpine:docker://node:current-alpine'
      - 'debian:docker://node:latest'
  container:
    docker_host: '/run/podman/podman.sock'
```

License
-------

MIT
