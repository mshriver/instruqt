---
slug: clientenrollment
id: aukyxvkq3qkd
type: challenge
title: Enrolling Client Systems
teaser: Add client systems to the realm
notes:
- type: text
  contents: |
    ## Enrolling Client Systems

    Client systems are typically Red Hat Enterprise Linux systems, however, they can be any
    UNIX or Linux systems that support the appropriate protocols. Red Hat Enterprise Linux
    systems are enrolled in the IdM kerberos realm using an installation script. This
    script covers all aspects of client configuration, including generating an identity for
    the client system. Once enrolled, the client system will be able to access all of the
    services of the realm, including authentication, policy, access control rules,
    authorization, etc..

    In this section, you will install the IdM client on a Red Hat Enterprise Linxu system
    and run the setup program to configure the system. You will verify the installation and
    that you can authenticate to the IdM realm and perform other actions.

    Please note that the Identity Management Web UI ***must*** launch in another browser
    tab or window. For security reasons, the IdM Web UI does not permit embedding in an iframe.
tabs:
- title: IdM Client
  type: terminal
  hostname: idmclient
difficulty: basic
timelimit: 5760
---
<!-- markdownlint-disable MD033 -->
## Welcome to challenge 2

### Preparation

Using the **IdM Client** tab, run the labsetup script:

```bash
/root/labsetup.sh
```
<pre class="file" style="white-space: pre-wrap; font-family:monospace;">
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/2)
</pre>

This sets up our client domain resolution and hostname. The client installation is now able to use DNS discovery to find our IdM server.

<hr>

### Exercise 2.1 - Install and configure the IdM client

In the IdM client tab, ensure that the client package is installed by running the following command:

```bash
yum -y install ipa-client
```

The client should already be installed. We are just making sure.

Now we can configure the client to use our IdM Server. During the setup you will be asked a number of questions, please accept the defaults, **unless outlined below**.

```bash
ipa-client-install --mkhomedir
```
Again, the mkhomedir command ensures that a home directory is created for the user at first login. An alternative is to use automount rules integrated with IdM.

<pre class="file" style="white-space: pre-wrap; font-family:monospace;">
This program will set up IPA client.
Version 4.10.0

Discovery was successful!
Do you want to configure chrony with NTP server or pool address? [no]:
</pre>

DNS service discovery detects our IdM server and you are asked if you want to specify a list of time servers to use with the client.

Type **no** or press Enter to accept the default.

We will use the system default time servers as they are reachable from the lab. Time synchronization is important in a kerberos environment as tickets are time limited to address various attacks. If time synchronization is too far out between the IdM server and clients, authentication failures occur.

Next, the discovered settings are displayed.

<pre class="file" style="white-space: pre-wrap; font-family:monospace;">
This program will set up IPA client.
Version 4.10.0

Discovery was successful!
Do you want to configure chrony with NTP server or pool address? [no]:
Client hostname: idmclient.[[ Instruqt-Var key="domain" hostname="idmserver" ]]
Realm: [[ Instruqt-Var key="realm" hostname="idmserver" ]]
DNS Domain: [[ Instruqt-Var key="domain" hostname="idmserver" ]]
IPA Server: idmserver.[[ Instruqt-Var key="domain" hostname="idmserver" ]]
BaseDN: dc=[[ Instruqt-Var key="sandbox_id" hostname="idmserver" ]],dc=instruqt,dc=io

Continue to configure the system with these values? [no]: <b>yes</b>
</pre>

Enter **yes** to continue to configure the system with the discovered values.

The installer synchronizes the time with the configured services and
displays the result. The installer then prompts you to enter the credentials of a user
authorised to enrol hosts.

<pre class="file" style="white-space: pre-wrap; font-family:monospace;">
Synchronizing time
No SRV records of NTP servers found and no NTP server or pool address was provided.
Using default chrony configuration.
Attempting to sync time with chronyc.
Time synchronization was successful.
User authorized to enroll computers:
</pre>

Use **admin** with the password you configured previously for the IdM Server.
```bash
redhat2023
```

<pre class="file" style="white-space: pre-wrap; font-family:monospace;">
User authorized to enroll computers: <b>admin</b>
Password for admin@[[ Instruqt-Var key="realm" hostname="idmserver" ]]:
</pre>

The client enrolment will now proceed without further input. The
configuration should only takes a few seconds. As soon as a system
is enrolled, all IdM services are available to the host and
to authenticated and authorized users.

<hr>

## Exercise 2.2 - Verify the client installation

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

Now that we have successfully configured the server and a client. Let's look at user management in more detail.
