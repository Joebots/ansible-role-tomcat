Tomcat
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-tomcat.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-tomcat)

Provides Apache Tomcat 7, 8 (default) or 9 for your system.

Context
-------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/robertdebock.github.io/artifacts/tomcat.png "Dependency")

Requirements
------------

These requirements can help you prepare your system for this role:
- robertdebock.bootstrap
- robertdebock.java
- robertdebock.haveged (optional, requires robertdebock.epel.)

Role Variables
--------------

You can install multiple instances and multiple versions. This configuration is defined in the variable "tomcat_layout". If you do not use the tomcat_layout, defaults will be used. See defaults/main.yml for the default values.

This is the default tomcat_layout:

```
tomcat_layout:
  - name: tomcat
    directory: /opt
    version: 8.5
    user: tomcat
    group: tomcat
    xms: 512M
    xmx: 1024M
    non_ssl_connector_port: 8080
    ssl_connector_port: 8443
    shutdown_port: 8005
    ajp_port: 8009
```

See "Example Playbooks" for futher details.

Dependencies
------------

These loose dependencies are available.

- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.java
- robertdebock.haveged

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.3|ansible 2.4|ansible 2.5|
|------------|-----------|-----------|-----------|
|alpine-3.6|yes|yes|yes|
|alpine-3.7|yes|yes|yes|
|archlinux|yes|yes|yes|
|centos-6|yes|yes|yes|
|centos-7|yes|yes|yes|
|debian-buster|yes|yes|yes|
|debian-stretch|yes|yes|yes|
|debian-wheezy|yes|yes|yes|
|fedora-26|yes|yes|yes|
|fedora-27|yes|yes|yes|
|opensuse-42.2|yes|yes|yes|
|opensuse-42.3|yes|yes|yes|
|ubuntu-artful|yes|yes|yes|
|ubuntu-xenial|yes|yes|yes|

Example Playbook
----------------

The simplest form:
```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.java
    - role: robertdebock.haveged
    - role: robertdebock.tomcat
```

And here is a heavily customized installation:
```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.java
    - role: robertdebock.haveged
    - role: robertdebock.tomcat
      tomcat_layout:
        - name: appone
          directory: /opt/appone
          version: 7
          user: appone
          group: appone
          xms: 512M
          xmx: 1024M
          non_ssl_connector_port: 8080
          ssl_connector_port: 8443
          shutdown_port: 8005
          ajp_port: 8009
          wars:
            - url: https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/sample.war
        - name: apptwo
          directory: /opt/apptwo
          version: 8
          user: tomcat
          group: tomcat
          xms: 512M
          xmx: 1024M
          non_ssl_connector_port: 8081
          ssl_connector_port: 8444
          shutdown_port: 8006
          ajp_port: 8010
        - name: appthree
          directory: /opt/appthree
          version: 9
          user: appthree
          group: appthree
          xms: 512M
          xmx: 1024M
          non_ssl_connector_port: 8082
          ssl_connector_port: 8445
          shutdown_port: 8007
          ajp_port: 8011
          wars_s3:
            - bucket: "example_bucket"
              object: "path/to/my.war"
```

Install this role using `galaxy install robertdebock.tomcat`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
