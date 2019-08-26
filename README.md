Kafka Ansible Role
=================
[![Build Status](https://travis-ci.org/mjcramer/ansible-role-kafka.svg?branch=master)](https://travis-ci.org/mjcramer/ansible-role-kafka) [![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-mjcramer.kafka-blue.svg)](https://galaxy.ansible.com/mjcramer/kafka/) 

An ansible role for installing Apache Kafka

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

Currently, the oracle and openjdk JVMs are supported. 

| Name | Value |
| --- | --- |
| kafka_version | 1.0, 2.0 |

Dependencies
------------

- mjcramer.system
- mjcramer.java

Tags
----
- apply
- configure
- initialize
- check


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: mjcramer.kafka, kafka_version: 1.0 }

License
-------

Unlicensed


Author Information
------------------

[Michael Cramer](http://michael.cramer.name), *michael@cramer.name* [_mjcramer_](http://github.com/mjcramer)
