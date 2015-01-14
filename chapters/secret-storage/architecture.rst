Architecture
============

This secret storage solution fits natively into any Salt infrastructure. This
means SaltStack provides the functionality for this service out of the box. It
simply requires some configuration and initial setup. The following sections
will outline each stop in the process.


Overview
--------

For a really high level overview here's how each of these components fit
together:

The Salt Master will be the central storage server. All secrets will be stored
within the pillar data on the master (usually ``/srv/pillar/``). Each secret is
stored on the master in a GPG encrypted cipher. The public and private GPG keys
are only ever stored on the master. GPG keys never need to be shared to
minions. The ``python-gnupg`` library needs to be installed on any minion
wanting to request secure keys. Lastly, assuming you'll want to secure your
encrypted secrets with a GPG key passphrase, you'll need a configured GPG agent
to unlock these secrets upon request, without the requirement of entering a
passphrase each time.


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
with root access to the general infrastructure. 

.. note::

    Anyone with root access on the SaltStack master will be able to retrieve any
    encrypted secret.


Pillar Data
-----------

The pillar system sits at the heart of this secret storage solution. This is
where the data is stored and where limitations on access to the data is
configured. In this example I assume the default path for pillar data,
``/srv/pillar``.

First, if it doesn't yet exist, create the pillar path:

.. code-block:: bash

    mkdir -p /srv/pillar

Second, populate a default ``top.sls`` file. This file is where you map data to
minions.

.. code-block:: yaml

    base:
      '*':
        - cache
      'alpha':
        - vault1
      'bravo':
        - vault2

In the example above I've made accessible anything in the ``cache.sls`` file to
all machines. The ``vault1.sls`` is accessible only to the host alpha, and
``vault2.sls`` only available to bravo. You'll of course want to update these
values to represent your own system names.


Salt Minion
-----------

The SaltStack minions will generally be the source for queries against the
secret store. By defining restrictions within the Pillar configuration you can
limit which machines have access to specific secrets or groups of secrets.
Secrets are queried from the minions using the ``pillar`` module. I've included
a few examples below:

The command below will query for the value of the ``secret1`` key. This key
represents a key-value store within one of the "vaults" available to this
system.

.. code-block:: bash

    salt-call pillar.item secret1

If you would like to request all secrets available to a specific system you can
use a similiar command:

.. code-block:: bash

    salt-call pillar.items

.. note::

    Different outputters can be used when retrieving these secrets.
    Outputters include ``--json``, ``--raw``, ``--txt``, ``yaml_out``, and more.
