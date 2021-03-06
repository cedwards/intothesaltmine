Key Management
==============

Before we can begin any communication on top of our ZeroMQ network we need to
accept encryption keys. The underlying ZeroMQ network is not encrypted, but
SaltStack adds a layer of AES public key encryption to all communications. This
adds very little overhead while ensuring that all communications are securely
encrypted between all hosts. Before these encryption keys are accepted on the
master, no communication will take place.

The Salt Master provides a utility called ``salt-key`` to allow you to manage
these encryption keys. Each minion will automatically generate their respective
keys and submit them to the master for acceptance. There are a number of ways
to manage keys at scale, but here we'll just look at the basic options of the
``salt-key`` utility.

``salt-key`` executes simple management of Salt public keys used for
authentication and encryption.

Listing keys
------------

.. code-block:: bash

    -l ARG, --list=ARG

The args ``pre``, ``un``, and ``unaccepted`` will list unaccepted/unsigned
keys.  The args ``acc`` or ``accepted`` will list accepted/signed keys.  The
args ``rej`` or ``rejected`` will list rejected keys.  Finally, ``all`` will
list all keys.

.. code-block:: bash

    -L, --list-all

List all public keys. (DEPRECATED: use ``--list-all``)

Accepting keys
--------------

.. code-block:: bash

    -a key_name, --accept=key_name

Accept the specified public key(s). Globs are supported.

.. code-block:: bash

    -A, --accept-all

Accept all pending keys.

.. code-block:: bash

    --include-all

Include non-pending keys when accepting or rejecting keys.


Rejecting keys
--------------

.. code-block:: bash

    -r key_name, --reject=key_name

Reject the specified public key. Globs are supported.

.. code-block:: bash

    -R, --reject-all

Reject all pending keys.

.. code-block:: bash

    --include-all

Include non-pending keys when accepting or rejecting keys.

Printing keys
-------------

.. code-block:: bash

    -p key_name, --print=key_name

Print the specified public key.

.. code-block:: bash

    -P, --print-all

Print all public keys.

Deleting keys
-------------

.. code-block:: bash

    -d key_name, --delete=key_name

Delete the specified key(s). Globs are supported.

.. code-block:: bash

    -D, --delete-all

Delete ALL keys.

Key fingerprints
----------------

.. code-block:: bash

    -f key_name, --finger=key_name

Print the specified key fingerprint.

.. code-block:: bash

    -F, --finger-all

Print all keys fingerprints.

Key Generation
--------------

.. code-block:: bash

    --gen-keys=key_name

Generate a keypair for use with Salt.

.. code-block:: bash

    --gen-keys-dir=/path/

Define the path to save the generated keypair. Only works with the
``--gen-keys`` option; default is the current directory.

.. code-block:: bash

    --keysize=key_size

Set the keysize for the generated key. Only works with the ``--gen-keys``
option. Keysize must be 2048 or higher; the default is 2048.
