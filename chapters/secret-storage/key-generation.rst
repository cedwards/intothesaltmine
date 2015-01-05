Key Generation
==============

This section outlines the steps required to generate the required public and
private keys.

.. code-block:: bash

    mkdir /etc/salt/gpgkeys
    chmod 0700 /etc/salt/gpgkeys
    
    gpg --homedir /etc/salt/gpgkeys --gen-key

At this point you'll be prompted with key generation options. 

 - Select ``(1) RSA and RSA (default)``.
 - Select ``2048`` for the keysize (default).
 - Select ``0`` for the key expiration (key does not expire).
 - Enter your project name for ``Real Name``.
 - Enter an email address for ``Email address``.
 - Enter a blank line for ``Comment``.

You'll be given one last chance to overview what you've entered and make any
changes. When you're ready to generate your keys, select ``(O)kay``.

At this point you'll be prompted for a passphrase. It should be noted that the
inclusion of a passphrase makes the overall configuration a bit more
complicated, but retains the highest amount of security. It will require a step
of manually unlocking the key with a passphrase anytime the server is rebooted.

Once the key is generated (it may take some time depending on your hardware)
you'll need to export the public key and import it into the systems keyring. To
export the public key you'll need to specify which key (as systems can have
many keys). This can be done using any unique information about the key,
including ``Real Name``, ``Email address`` or ``Comment`` as defined during the
key generation. The example below exports the key using the email address.

.. code-block:: bash

    gpg --homedir /etc/salt/gpgkeys --armor --export email@address.org > pubkey.gpg

.. code-block:: bash

    gpg --import pubkey.gpg

You are now ready to encrypt data using this GPG key-pair. This can be done
using a simple shell one-liner:

.. code-block:: bash

    echo -n "top secret data" | gpg --homedir /etc/salt/gpgkeys --armor --encrypt -r email@address.org

The resulting output of this can then be used within the SaltStack pillar
system in the following format:

.. code-block:: yaml

    secret: |
      -----BEGIN PGP MESSAGE-----
      Version: GnuPG v1

      hQEMAweRHKaPCfNeAQf9GLTN16hCfXAbPwU6BbBK0unOc7i9/etGuVc5CyU9Q6um
      QuetdvQVLFO/HkrC4lgeNQdM6D9E8PKonMlgJPyUvC8ggxhj0/IPFEKmrsnv2k6+
      cnEfmVexS7o/U1VOVjoyUeliMCJlAz/30RXaME49Cpi6No2+vKD8a4q4nZN1UZcG
      RhkhC0S22zNxOXQ38TBkmtJcqxnqT6YWKTUsjVubW3bVC+u2HGqJHu79wmwuN8tz
      m4wBkfCAd8Eyo2jEnWQcM4TcXiF01XPL4z4g1/9AAxh+Q4d8RIRP4fbw7ct4nCJv
      Gr9v2DTF7HNigIMl4ivMIn9fp+EZurJNiQskLgNbktJGAeEKYkqX5iCuB1b693hJ
      FKlwHiJt5yA8X2dDtfk8/Ph1Jx2TwGS+lGjlZaNqp3R1xuAZzXzZMLyZDe5+i3RJ
      skqmFTbOiA==
      =Eqsm
      -----END PGP MESSAGE-----
      
    ...
