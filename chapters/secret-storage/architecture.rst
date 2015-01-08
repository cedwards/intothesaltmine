Architecture
============

This secret storage solution fits natively into any Salt infrastructure. This
means SaltStack provides the functionality for this service out of the box. It
simply requires some configuration and initial setup. The following sections
will outline each stop in the process.

Salt Master
-----------

The secret store is housed on the SaltStack master within the Pillar system.
This allows for the secrets to be available to any connected system, with
access limitations defined by the Pillar top file. In some of my early
discussions about this architecture there was some concern about the privacy
and general access to the secret store server. It should be noted that the
SaltStack master should restrict general access just as any "vault" would. If
you can't trust your administrators that have root access to your systems with
the secrets stored on your server, well, I'd say you have bigger problems. This
configuration assumes access to the SaltStack master is restricted to those
with root access to the general infrastructure. Anyone with root access on the
SaltStack master will be able to retrieve any encrypted secret.

Salt Minion
-----------

The SaltStack minions will generally be the source for queries against the
secret store. By defining restrictions within the Pillar configuration you can
limit which machines have access to specific secrets or groups of secrets.
Secrets are queried from the minions using the ``pillar`` module. I've included
a few examples below:

The command below will query for the value of the ``secret1`` key:

.. code-block:: bash

    salt-call pillar.item secret1

If you would like to request all secrets available to a specific system you can
use a similiar command:

.. code-block:: bash

    salt-call pillar.items

Note: different outputters can be used when retrieving these secrets.
Outputters include ``--json``, ``--raw``, ``--txt``, ``yaml_out``, and more.
