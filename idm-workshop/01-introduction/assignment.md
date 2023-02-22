---
slug: introduction
id: xdwt3vpkpgdu
type: challenge
title: Install the Identity Management Server
teaser: Introduction and Install the Server.
notes:
- type: text
  contents: |
    ## Introduction

    Identity Management (IdM) in Red Hat Enterprise Linux provides a centralized
    and unified way to manage identity stores, authentication, policies, and authorization
    policies in a Linux-based domain.  IdM significantly reduces the administrative
    overhead of managing different services individually and using different tools
    on different machines.

    In this workshop you will learn how to deploy IdM servers
    and enrol client machines, define and manage user and service identities, set
    up access policies, configure network services to take advantage of IdM's authentication
    and authorisation facilities and issue X.509 certificates for services.

    IdM is based on the upstream project FreeIPA integrated and is integrated with the Fedora distribution. FreeIPA
    itself is based on 389ds directory server, dogtag certificate system and MIT kerberos. IdM leverages
    sssd within Red Hat Enterprise Linux.
tabs:
- title: IdM Server
  type: terminal
  hostname: idmserver
- title: IdM Web UI
  type: external
  url: https://idmserver.${_SANDBOX_ID}.instruqt.io
difficulty: basic
timelimit: 3600
---
<!-- markdownlint-disable MD033 -->
The lab environment consists of the following

- 1 Red Hat Identity Management (IdM) server host
- 1 IdM replica server
- 1 IdM client

You are logged into the consoles with root access by the environment. A local user account has been created with sudo privelige that you can use:

username: rhel
password: redhat

This user is a member of wheel and has sudo access.

Once we have established our IdM installation we will no longer use this account. In a production environment, this account would be disabled or removed. In highly secure environments, the root password would also be randomized. In the event that there were a problem with the IdM configuration, the root password would have to be reset. Normally we run multiple replicas for the IdM primary servers to ensure high availability.


## <ins>Preparation</ins>

Using the <b>IdM Server</b> tab, run:

```bash
/root/labsetup.sh
```

This sets up our lab hostname and domain. Ensure that you can ping the hostname and that hostnamectl returns the proper name. NOTE: currently nslookup will fail as the idm server is not up.

## <ins>Install the IdM server</ins>

Using the <b>IdM Server</b> tab, run the server installation. We add the ``--no-host-dns`` parameter to support our lab environment. In a production environment to ensure proper host resolution checking the host dns entry ensures that upstream resolution is working properly. The ``--mkhomedir`` option is used to ensure that PAM creates missing home directories for users when the login for the first time. IdM also supports automount for those use cases where its use is authorized.

You will be asked a number of questions during the installation, please accept the defaults, <b> unless outlined below </b>

```bash
sudo ipa-server-install --no-host-dns --mkhomedir
```

We want the server to manage our DNS for us. Answer ``yes`` to the following question:

Configure FreeIPA's DNS server::
```bash
Do you want to configure integrated DNS (BIND)? [no]: yes
```
Kerberos is tightly tied to DNS as the protocol uses DNS for service discovery. In a DNS zone, there is only one kerberos authority allowed. The service entries for kerberos accept multiple values, however, all systems must belong to the same realm. If there is an Active Directory service running in the same DNS domain we will have a conflict and authentication will randomly fail as there are multiple authorities. IdM servers and AD servers must belong to separate DNS domains. Typically, AD DNS services delegate to IdM servers for the IdM domain.

Accept default values for the server hostname, domain name and realm::

```bash
Server host name [idmserver.idmworkshop.lab]:

Warning: skipping DNS resolution of host idmserver.idmworkshop.lab
The domain name has been determined based on the host name.

Please confirm the domain name [idmworkshop.lab]:

The kerberos protocol requires a Realm name to be defined.
This is typically the domain name converted to uppercase.

Please provide a realm name [IDMWORKSHOP.LAB]:
```

You will need to provide a password for the LDAP Directory Manager and
for the IdM admin account. Please use:

```bash
redhat2023
```

You will be prompted to confirm the installation with the stated
configuration. Answer ``yes``

```
Continue to configure the system with these values? [no]: yes
```

The installation should take about 5-10 minutes. Follow the
remainder of the installation process and examine the output.
The installation will:
* Configure a stand-alone CA (dogtag) for certificate management
* Configure the NTP client (chronyd)
* Create and configure an instance of Directory Server
* Create and configure a Kerberos Key Distribution Center (KDC)
* Configure Apache (httpd)
* Configure SID generation
* Configure the KDC to enable PKINIT

Please examine the notes at the end of the installation and take
note of the firewall ports that are required to be open to the
IdM Server system.

From a host-based firewall prespective (always recommended),
the network ports have been setup for you as part of the lab
preparation using firewalld. You can examine the configuration
with firewall-cmd. Red Hat provides preconfigured service
descriptions for easily enabling the appropriate ports for IdM.
Running list-services, you should see a list similar to the following:

```bash
firewall-cmd --list-services
cockpit dhcpv6-client dns freeipa-4 freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ssh
```

To investigate what is controlled by each service listing, run a command similar to the
following:

```bash
firewall-cmd --info-service freeipa-ldaps
```
```
freeipa-ldaps
  ports: 80/tcp 443/tcp 88/tcp 88/udp 464/tcp 464/udp 123/udp 636/tcp
```
```bash
firewall-cmd --info-service freeipa-4
```
```
freeipa-4
  ports:
  protocols:
  source-ports:
  modules:
  destination:
  includes: http https kerberos kpasswd ldap ldaps
  helpers:
```

Verify that you can authenticate with the new IdM realm using the admin user and the password you provided.

```bash
kinit admin@IDMWORKSHOP.LAB
```

NOTE: you can omit the realm name as it will be setup as a default in /etc/krb5.conf


Run klist to verify your authenitication token.
```bash
klist
```
