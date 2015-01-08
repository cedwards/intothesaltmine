Requirements
============

The set of requirements defined for this project are outlined below. These
requirements span a miriad of topics ranging from encryption to scalability.

Encryption
----------

 - Secrets not stored in plain text on disk (client or server)
 - Secrets always encrypted in transit across network

Reliability
-----------

 - Fault tolerant enough to have the server go down and the application keeps functioning
 - Server uptime equivalent to the most critical server or component in the data center

Scalability
-----------

 - Can support 30,000 concurrent servers
 - Can support access of over 300,000 secrets

Secret types
------------

 - Support standard secret types (ssh, key pairs, db, ldap, etc)
 - Custom secrets with custom key/value meta data
 - Support for automated changing of passwords on schedules and on demand

Administration
--------------

 - Searching across all fields
 - APIs for administration of secrets from the command line
 - Bulk edit of secrets
 - SDKs for integrating with code and cli
 - CLI utility

Access Control
--------------

 - Secrets can be permissioned to individual servers
 - Read, write, and delete as separate permissions
 - Group permissioning for both secrets and applications/clients

Required Packages
=================

In order to build this secret storage solution you'll need:

 - Salt Master
 - Salt Minion(s)
 - ``python-gnupg`` installed
 - Public and private GPG keys
 - GPG Agent

For a really high level overview here's how each of these components fit
together.

The Salt Master will be the central storage server. All secrets will be stored
within the pillar data on the master (usually ``/srv/pillar/``). Each secret is
stored on the master in a GPG encrypted cipher. The public and private GPG keys
are only ever stored on the master. GPG keys never need to be shared to
minions. The ``python-gnupg`` package needs to be installed on any minion
wanting to request secure keys. Lastly, assuming you'll want to secure your
encrypted secrets with a GPG key passphrase, you'll need a configured GPG agent
to unlock these secrets upon request, without the requirement of entering a
passphrase each time.
