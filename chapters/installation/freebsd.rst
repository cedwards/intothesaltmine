FreeBSD
=======

SaltStack is available for FreeBSD in both package and port form. Outlined
below are instructions on installing and starting the Salt service(s) on
FreeBSD.

Installation
------------

On FreeBSD 10 and later Salt can be installed using the ``pkgng`` utility:

.. code-block:: bash

    pkg install py27-salt

On older systems, or systems not using pre-compiled packages, compilation from
ports is also available:

.. code-block:: bash

    make -C /usr/ports/sysutils/py-salt install clean

Either of these methods will install the full set of Salt utilities including
the Salt master, minion, syndic. Repeat the above instructions for any FreeBSD
system you'd like to be part of your Salt infrastructure.

Post-Installation
-----------------

The FreeBSD port for Salt lays down a sample config for both master and minion.
While the service will technically run using only default values without a
config file in place, you'll likely want to copy the sample config into use.

**Master**

Copy the sample config file:

.. code-block:: bash

    cp /usr/local/etc/salt/master.sample /usr/local/etc/salt/master

**rc.conf**

Activate the Salt master in ``/etc/rc.conf``:

.. code-block:: diff

    + salt_master_enable="YES"

**Start the Master**

Start the Salt master:

.. code-block:: bash

    service salt_master start

**Minion**

Copy the sample config file:

.. code-block:: bash

    cp /usr/local/etc/salt/minion.sample /usr/local/etc/salt/minion

**rc.conf**

Activate the Salt minion in ``/etc/rc.conf``:

.. code-block:: diff

    + salt_minion_enable="YES"

**Start the Minion**

Start the Salt minion:

.. code-block:: bash

    service salt_minion start
