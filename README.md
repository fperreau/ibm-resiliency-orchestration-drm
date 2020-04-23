Role Name
=========

IBM Resiliency Orchestration (RO) DRM role help you to install Disaster Recovery manager in Linux Red Hat server.

Requirements
------------

All you need is to download tIBM Resiliency Orchestration server file from IBM Support Fix Central web site https://www.ibm.com/support/fixcentral and the Open Source prerequisits:
  
  ## download web sites URI
    - https://www.ibm.com/support/fixcentral
    - https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.27/
    - https://downloads.mariadb.org/mariadb/10.3.17/
    - https://sourceforge.net/projects/gnu-utils/files/binaries/


Role Variables
--------------

Default role variable are liste bellow. Those variables define the Binary files needed for IBM Resiliency Orchestration and the prerequisit Open Source software.

  ## Files/repository content the different binary files
    FILES:        "DRM_8.0.4.0"

  ## Temporary directory use to install DRM
    BUILD:        "/tmp/build.Server"
  
  ## Default paramters for IBM Resiliency Orchestation, Apache Tomcat, MaraiDB
    EAMSROOT:     /opt/panaces
    TOMCAT_HOME:  /opt/tomcat9
    SQLID:        99
    SQLPORT:      3306
    SQLPASS:      "secret"
    IPDRM:        "{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"

  ## List of files needed to install IBM Resiliency Orchestration version 8.0.4.0
    BINARY_FILES:
    - { archive: Server.tar.gz,  creates: install.bin }
    - { archive: MariaDB.tgz,    creates: MariaDB-server-10.3.17-1.el8.x86_64.rpm }
    - { archive: Tomcat.tgz,     creates: apache-tomcat-9.0.27.tar.gz }
    - { archive: ThirdParty.tgz, creates: ThirdPartyJSLib.zip }

  ## Templates files used in IBM Resiliency Orchestration install
    TEMPLATE_FILES:
    - PanacesServerInstaller.properties
    - panaces.service

  ## Detail of MariaDB packages content of "MariaDB.tgz"
    MARIADB_PKG:
    - "{{BUILD}}/MariaDB-common-10.3.17-1.el8.x86_64.rpm"
    - "{{BUILD}}/MariaDB-client-10.3.17-1.el8.x86_64.rpm"
    - "{{BUILD}}/MariaDB-server-10.3.17-1.el8.x86_64.rpm"

  ## Detail of Apache Tomcat binary file of "Tomcat.tgz"
    TOMCAT_SRC: "apache-tomcat-9.0.27"
    TOMCAT_PKG: "{{BUILD}}/{{TOMCAT_SRC}}.tar.gz"

  ## Detail of third party Open Source software content of "ThirdParty.tgz"
    THIRDPARTY_JSLIB:   "{{BUILD}}/ThirdPartyJSLib.zip"
    THIRDPARTY_GNULIB:  "{{BUILD}}/gnulib.zip"


Dependencies
------------

Minimal resources needed by server to run IBM Resiliency Orchestration are:

    - CPU:  4
    - RAM:  8 GiB
    - DISK: 50 GiB + 100 GiB
    - OS:   RHELv8

We need to download the IBM Resiliency Orchestration server and Open Source prerequisits in Ansible files directory name to DRM_8.0.4.0 for this version 8.0.4.0.

    - files:
      - DRM_8.0.4.0:
        - Server.tar.gz           # binary tar file of IBM Resiliency Orchestration server
        - MariaDB.tgz             # binary tar file contents the MariaDB 10.3.17 RPM packages
        - Tomcat.tgz              # binary tar file contents the Apache Tomcat version 9.0.27 tar file
        - ThirdParty.tgz          # binary tar file contents the third party open source software
  
For dependency Red Hat packages, you can use the **Red Hat BaseOS+AppStream** yum repository from RHELv8 ISO distribution. 

Example DRM Playbook
--------------------

You can install IBM Resiliency Orchestration server with this simple deploy **site.yml** example of DRM Ansible roles.

Execute this simple command **ansible-playbook site.yml** to deploy your first IBM Resiliency Orchestration DR Manager on Linux Red Hat server.

    - name: Install RESILIENCY ORCHESTRATION
      hosts: DRM
      remote_user: root
      roles:
        - fperreau.ibm_resiliency_orchestration_drm

License
-------

MIT, Apache-2.0

Author Information
------------------

I am a Resiliency Senior Architect in IBM Resiliency Services France. I work on the automate Disaster Recovery Plans to help Business to be more resilient. I love the Simplicity of Ansible, the Python/YAML/Jinja2 languages, Linux and Open Sources software.
