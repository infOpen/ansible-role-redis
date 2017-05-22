# redis

[![Build Status](https://travis-ci.org/infOpen/ansible-role-redis.svg?branch=master)](https://travis-ci.org/infOpen/ansible-role-redis)

Install redis package.

## Requirements

This role requires Ansible 2.0 or higher,
and platform requirements are listed in the metadata file.

## Testing

This role use [Molecule](https://github.com/metacloud/molecule/) to run tests.

Locally, you can run tests on Docker (default driver) or Vagrant.
Travis run tests using Docker driver only.

Currently, tests are done on:
- Debian Jessie
- Ubuntu Trusty
- Ubuntu Xenial

and use:
- Ansible 2.0.x
- Ansible 2.1.x
- Ansible 2.2.x
- Ansible 2.3.x

### Running tests

#### Using Docker driver

```
$ tox
```

#### Using Vagrant driver

```
$ MOLECULE_DRIVER=vagrant tox
```

## Role Variables

### Default role variables

``` yaml
# Repository management
redis_repository_cache_valid_time: 3600

redis_repository_keyserver: "{{ _redis_repository_keyserver | default('') }}"
redis_repository_key_id: "{{ _redis_repository_key_id | default('') }}"
redis_repositories: "{{ _redis_repositories | default([]) }}"

# Package management
redis_packages: "{{ _redis_packages }}"
redis_not_use_system_version: True

# Service management
redis_server_service_name: "{{ _redis_server_service_name }}"
redis_server_service_state: 'started'
redis_server_service_enabled: True

# Files configuration
#--------------------
redis_folders: "{{ _redis_folders }}"
redis_server_config_file:
  name: 'redis.conf'
  owner: 'redis'
  group: 'redis'
  mode: '0600'

# General server configuration
redis_server_daemonize: 'yes'
redis_server_pid_file: "{{ redis_folders.pid.path }}/redis-server.pid"
redis_server_port: 6379
redis_server_bind_ip:
  - 127.0.0.1
redis_server_use_unixsocket: False
redis_server_unixsocket: "{{ redis_folders.socket.path }}/redis.sock"
redis_server_unixsocketperm: 755
redis_server_timeout: 0
redis_server_tcp_keepalive: 0
redis_server_loglevel: 'notice'
redis_server_logfile: "{{ redis_folders.log.path }}/redis-server.log"
redis_server_use_syslog: 'no'
redis_server_syslog_enabled: 'no'
redis_server_syslog_ident: 'redis'
redis_server_syslog_facility: 'local0'
redis_server_database: 16

# Snapshotting server configuration
redis_server_save:
  - '900 1'
  - '300 10'
  - '60 10000'
redis_server_stop_writes_on_bgsave_error: 'yes'
redis_server_rdbcompression: 'yes'
redis_server_rdbchecksum: 'yes'
redis_server_dbfilename: 'dump.rdb'
redis_server_dir: "{{ redis_folders.data.path }}"

# Replication configuration
redis_server_slaveof: False
redis_server_slaveof_master_ip: ''
redis_server_slaveof_master_port: ''
redis_server_master_auth: ''
redis_server_slave_serve_stale_data: 'yes'
redis_server_slave_read_only: 'yes'
redis_server_repl_ping_slave_period: 10
redis_server_repl_timeout: 60
redis_server_repl_disable_tcp_nodelay: 'no'
redis_server_repl_backlog_size: '1mb'
redis_server_repl_backlog_ttl: 3600
redis_server_slave_priority: 100
redis_server_min_slaves_to_write: 3
redis_server_min_slaves_max_lag: 10

# Security configuration
redis_server_require_pass: ''
redis_server_renamed_commands: []

# Limits configuration
redis_server_maxclients: 10000
redis_server_limit_maxmemory: False
redis_server_limit_maxmemory_size: 4000000000
redis_server_maxmemory_policy: 'volatile-lru'
redis_server_maxmemory_samples: 3

# Append only mode configuration
redis_server_appendonly: 'no'
redis_server_appendfilename: 'appendonly.aof'
redis_server_appendfsync: 'everysec'
redis_server_no_appendfsync_on_rewrite: 'no'
redis_server_auto_aof_rewrite_percentage: 100
redis_server_auto_aof_rewrite_min_size: '64mb'

# Lua scripting configuration
redis_server_lua_time_limit: 5000

# Slow log configuration
redis_server_slowlog_log_slower_than: 10000
redis_server_slowlog_max_len: 128

# Event notification configuration
redis_server_notify_keyspace_events: ''

# Advanced configuration
redis_server_hash_max_ziplist_entries: 512
redis_server_hash_max_ziplist_value: 64
redis_server_list_max_ziplist_entries: 512
redis_server_list_max_ziplist_value: 64
redis_server_set_max_intset_entries: 512
redis_server_zset_max_ziplist_entries: 128
redis_server_zset_max_ziplist_value: 64
redis_server_activerehashing: 'yes'
redis_server_client_output_buffer_limit:
  - 'normal 0 0 0'
  - 'slave 256mb 64mb 60'
  - 'pubsub 32mb 8mb 60'
redis_server_hz: 10
redis_server_aof_rewrite_incremental_fsync: 'yes'

# Includes
redis_server_include: []
```

### Debian OS family variables

``` yaml
# Packages management
_redis_packages:
  - name: 'redis-server'

# Service management
_redis_server_service_name: 'redis-server'

# Redis paths
_redis_folders:
  config:
    path: '/etc/redis'
  data:
    path: '/var/lib/redis'
  log:
    path: '/var/log/redis'
  pid:
    path: '/var/run/redis'
  socket:
    path: '/var/run/redis'
```

### Ubuntu distributions variables

``` yaml
_redis_repository_keyserver: 'keyserver.ubuntu.com'
_redis_repository_key_id: 'C7917B12'
_redis_repositories:
  - repo: >
      deb http://ppa.launchpad.net/chris-lea/redis-server/{{ ansible_distribution | lower }}
      {{ ansible_distribution_release }}
      main
  - repo: >
      deb-src http://ppa.launchpad.net/chris-lea/redis-server/{{ ansible_distribution | lower }}
      {{ ansible_distribution_release }}
      main
```

## Dependencies

None

## Example Playbook

``` yaml
- hosts: servers
  roles:
    - { role: infOpen.redis }
```

## License

MIT

## Author Information

Alexandre Chaussier (for Infopen company)
- http://www.infopen.pro
- a.chaussier [at] infopen.pro
