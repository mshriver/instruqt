---
slug: hostbasedaccesscontrol
id: eh5ktd4isetr
type: challenge
title: Host Based Access Control
teaser: Controlling user access to systems
notes:
- type: text
  contents: |
    ## Host Based Access Control

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

