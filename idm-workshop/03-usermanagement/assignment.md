---
slug: usermanagement
id: k9cirezut12m
type: challenge
title: User Management and Kerberos Authentication
teaser: Create and Manage users, groups and services
notes:
- type: text
  contents: |
    ## User Management and Kerberos Authentication

    Once our Identity Management servers are configured, we can start creating the objects that
    we use to manage our realm. These include users, user groups, hosts, host groups and services.
    IdM uses a directory server backend and can store all the pertinent information required for
    complete user management - user facts, user authentication methods, ssh keys, certificates,
    login shell and home directory.

    We are also able to configure elements like password policy, SELinux policy, etc.. We will explore
    these in more detail in later exercises.

    In the challenge we will use both the commandline interface and the web interface to IdM
    to configure user groups, users and to manage their authentication to the systems.
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

Using the <b>IdM Client</b> tab, run:

```bash
/root/labsetup.sh
```

This sets up our client domain resolution and hostname. This allows the client installation to use discovery to find our IdM server.


## <ins>Install the IdM client</ins>

In the IdM client tab, ensure that the client package is installed by running the following command:

```bash
yum -y install ipa-client
```

Now lets make sure that the client is configured. You will be asked a number of questions during the installation, please accept the defaults, <b> unless outlined below </b>

```bash
ipa-client-install
```



