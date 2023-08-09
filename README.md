
# CP-Ansible

## Introduction

Ansible provides a simple way to deploy, manage, and configure the Confluent Platform services. This repository provides playbooks and templates to easily spin up a Confluent Platform installation. Specifically this repository:

* Installs Confluent Platform packages or archive.
* Starts services using systemd scripts.
* Provides configuration options for many security options including encryption, authentication, and authorization.

The services that can be installed from this repository are:

* ZooKeeper
* Kafka
* Schema Registry
* REST Proxy
* Confluent Control Center
* Kafka Connect (distributed mode)
* KSQL Server
* Replicator


## non-root 설치

### 사전 준비해야 할 것
- 사용자/그룹 생성
- ansible 사용하기 위한 사용자의 authorized_keys 에 public key 추가
- Confluent Platform 설치 (data, log, binary, configuration)를 위한 디렉토리
- JDK 17 

### hosts.yml (예)
```
ansible_user: confluent
ansible_become: false

deployment_user: confluent
deployment_group: confluent
deployment_path: /opt/confluent

install_java: false

kafka_connect_custom_properties:
  plugin.path: {{ deployment_path }}/connect_plugins

zookeeper_log_dir: {{ deployment_path }}/log/zookeeper
zookeeper_custom_properties:
  dataDir: {{ deployment_path }}/data/zookeeper
  dataLogDir : {{ deployment_path }}/log/zookeeper

kafka_broker_log_dir: {{ deployment_path }}/log/kafka
kafka_broker_custom_properties:
  log.dirs: {{ deployment_path }}/data/kafka

kafka_connect_log_dir: {{ deployment_path }}/log/kafka-connect
kafka_connect_custom_properties:
  plugin.path: {{ deployment_path }}/connect_plugins

ksql_rocksdb_path: {{ deployment_path }}/data/ksqldb-rockdb
ksql_log_dir: {{ deployment_path }}/log/ksql
ksql_custom_properties:
  ksql.streams.state.dir: {{ deployment_path }}/data/ksqldb-streams

schema_registry_log_dir: {{ deployment_path }}/log/schema-registry

control_center_rocksdb_path: {{ deployment_path }}/data/control-center-rocksdb
control_center_log_dir: {{ deployment_path }}/log/control-center
control_center_custom_properties:
  confluent.controlcenter.data.dir: {{ deployment_path }}/data/control-center
```

## Documentation

You can find the documentation for running CP-Ansible at https://docs.confluent.io/current/installation/cp-ansible/index.html.

You can find supported configuration variables in [VARIABLES.md](docs/VARIABLES.md)

## Contributing

If you would like to contribute to the CP-Ansible project, please refer to the [CONTRIBUTE.md](docs/CONTRIBUTING.md)


## License

[Apache 2.0](docs/LICENSE.md)
