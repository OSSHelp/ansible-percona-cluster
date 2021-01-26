# Percona XtraDB Cluster

| master |
| -------- |
| [![Build Status](https://drone.osshelp.ru/api/badges/ansible/percona-cluster/status.svg)](https://drone.osshelp.ru/ansible/percona-cluster) |

Role which installs and configures Percona XtraDB Cluster.

## Deploy example (do not copy blindly!)

```yaml
    - role: percona-cluster
      percona_cluster_version: "5.7"
      percona_cluster_root_password: "strong_password_from_vault"
      percona_cluster_general:
        datadir: /var/lib/mysql
        bind_address: '0.0.0.0'
        server_id: 'some_unique_id'
        innodb_large_prefix: false
        expire_log_days: 8
      percona_cluster_wsrep:
        primary: true
        wsrep_cluster_address: "gcomm://1.1.1.1,2.2.2.2,3.3.3.3"
        wsrep_cluster_name: 'test-cluster'
        wsrep_node_address: "1.2.3.4"
        wsrep_node_name: 'some-node'
        wsrep_provider: "/usr/lib/libgalera_smm.so"
        wsrep_sst_method: "xtrabackup-v2"
        wsrep_slave_threads: '1'
        wsrep_provider_options: ''
        sst_username: 'some-username'
        sst_password: 'another_strong_password_from_vault'
        monitoring_username: 'netdata'
        monitoring_password: 'and_another_strong_password_from_vault'
      percona_cluster_wsrep_notify:
        enabled: true
        script_path: '/usr/local/sbin/custom.wsrep_notify'
        mail_addr: 'project-name-here@osshelp.pagerduty.com'
```

## About available parameters

### Main params

| Param | Description |
| -------- | -------- |
| percona_cluster_version | version of Percona-cluster to install|
| percona_cluster_root_password | root password for MySQL. Make sure it is taken from vault|

### percona_cluster_general

These params are for setting up general MySQL params:

| Param | Description |
| -------- | -------- |
| datadir | MySQL data directory |
| bind_address | an IP-address to bind to |
| server_id | unique id, needed for correct clusterization |
| innodb_large_prefix | when set to "true", index key prefixes longer than 767 bytes (up to 3072 bytes) are allowed for InnoDB |
| expire_log_days | number of days to keep binary logs |

### percona_cluster_wsrep

Most of these params are for setting up wsrep, check [our article](https://oss.help/kb278) for more details. But there are some extra:

| Param | Description |
| -------- | -------- |
| percona_cluster_version | version of Percona-cluster to install |
| percona_cluster_root_password | root password for MySQL. Make sure it is taken from vault |
| primary | if set to "true" the node will be used for sst-user and monitoring-user creation |
| monitoring_username, monitoring_password | credentials for monitoring access (netdata) |

### percona_cluster_wsrep_notify

Params prior to our script for checking cluster status. See [this article](https://oss.help/kb2176) for info. Make sure to use the correct address in mail_addr variable (Pagerduty address), or we won't receive those alerts when cluster breaks.

## Cluster bootstraping

Not supported by the role, and there is no assurance that it will be implemented. You need to bootstrap manually.

## Useful links

- [Official documentation](https://www.percona.com/software/documentation)
- [Our article about cluster setup](https://oss.help/kb278)
- [Our article about monitoring script (wsrep_notify)](https://oss.help/kb2176)

## TODO

- Check the role in various builds
- Improve tests (separate testing of different versions)
