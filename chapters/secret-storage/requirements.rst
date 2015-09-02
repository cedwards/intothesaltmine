Requirements
============

The set of requirements defined for this project are outlined below.

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

 - Can support thousands of concurrent servers
 - Can support storage of thousands of secrets

Secret types
------------

 - Support standard secret types (PKI key pairs, hashes, etc)
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
-----------------

In order to build this secret storage solution you'll need:

 - Salt Master
 - Salt Minion(s)
 - Public and private GPG keys
 - GPG Agent (optional)

Required Configuration
----------------------

In order for Salt master to parse the GPG cipher data it needs the GPG
renderer enabled. This is done by updating the ``/etc/salt/master`` config file
and applying the below change.

.. code-block:: diff

    - #renderer: yaml_jinja
    + renderer: jinja | yaml | gpg

