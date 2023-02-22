---
slug: clientenrollment
id: aukyxvkq3qkd
type: challenge
title: Enrol Client Systems
teaser: Add client systems to the realm
notes:
- type: text
  contents: |
    ## Enrolling Client Systems

    Client systems are enrolled in the IdM kerberos realm using an installation script. This
    script covers all aspects of client configuration, including generating an identity for
    the client system. Once enrolled, the client system will be able to access all of the
    services of the realm, including authentication, policy, access control rules,
    authorization, etc..

    In this section, you will install the IdM client on one of the systems and run the
    setup to configure the system. You will verify that you can authenticate to the
    IdM realm and perform other actions.
tabs:
- title: IdM Server
  type: terminal
  hostname: idmserver
- title: IdM Web UI
  type: external
  url: https://idmserver.${_SANDBOX_ID}.instruqt.io
- title: IdM Client
  type: terminal
  hostname: idmclient
- title: IdM Replica
  type: external
  url: https://idmclient.${_SANDBOX_ID}.instruqt.io:9090
- title: IdM Replica
  type: terminal
  hostname: idmreplica
difficulty: basic
timelimit: 3600
---
<!-- markdownlint-disable MD033 -->
Welcome to challenge 2

## <ins>Preparation</ins>

Using the <b>IdM Client</b> tab, run the labsetup script:

```bash
/root/labsetup.sh
```

This sets up our client domain resolution and hostname. The client installation is now able to use discovery to find our IdM server.


## <ins>Install the IdM client</ins>

In the IdM client tab, ensure that the client package is installed by running the following command:

```bash
yum -y install ipa-client
```

Now we can configure the client to use our IdM Server. During the setup you will be asked a number of questions, please accept the defaults, <b> unless outlined below </b>

```bash
ipa-client-install --mkhomedir
```
DNS service discovery detects our IdM server and the settings are displayed; enter ``yes`` to proceed.

  Discovery was successful!
  Client hostname: idmclient.idmworkshop.lab
  Realm: IDMWORKSHOP.LAB
  DNS Domain: idmworkshop.lab
  IPA Server: idmserver.idmworkshop.lab
  BaseDN: dc=idmworkshop,dc=lab

  Continue to configure the system with these values? [no]: yes

Next, you are asked if you want to provide a list of time servers
to synchonized the client's time. Select the default ``no``. Time
synchronization is important in a kerberos environment as tickets
are time limited to address various attacks. We are using the
default time server pools configured by the OS.

The installer will prompt you to enter the credentials of a user
authorised to enrol hosts. Use ``admin`` with the password you
configured previously::

  User authorized to enroll computers: admin
  Password for admin@IDMWORKSHOP.LAB:

The client enrolment will now proceed without further input. The
configuration of the client enrolment should only takes a few seconds.

As soon as a system is enrolled, all IdM services are available to
the host and to authenticated and authorized users.

Verify that your system has access to the realm by authenicating again as
admin

```bash
kinit admin
```
No output is good news.

```bash
klist
```

You should see the kerberos principal information displayed.