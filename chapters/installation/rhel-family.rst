Red Hat Enterprise
==================

Salt can be installed on Red Hat Enterprise (and variants) using the EPEL
repository. This additional repository is maintained primarily by Red Hat
employees and Fedora contributors. It contains additional enterprise packages
for use with Red Hat and its variants.

Enable EPEL
-----------

To enable the EPEL repository install the appropriate package listed below
based on your version of Red Hat.

**RHEL 5**

.. code-block:: bash

    rpm -Uvh http://mirror.pnl.gov/epel/5/i386/epel-release-5-4.noarch.rpm

**RHEL 6**

.. code-block:: bash

    rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/6/i386/epel-release-6-8.noarch.rpm

Installation
------------

On Red Hat based systems the Salt master, minion and syndic packages are built
seperately. It is necessary to install the appropriate package for the system
role. Typically this means you'll have one Salt master and many Salt minions.

**salt-master**

.. code-block:: bash

    yum install salt-master

**salt-minion**

.. code-block:: bash

    yum install salt-minion

Post Installation
-----------------

**Master**

Configure the service to start at boot:

.. code-block:: bash

    chkconfig salt-master on

Start the service:

.. code-block:: bash

    service salt-master start

**Minion**

Configure the service to start at boot:

.. code-block:: bash

    chkconfig salt-minion on

Start the service:

.. code-block:: bash

    service salt-minion start
