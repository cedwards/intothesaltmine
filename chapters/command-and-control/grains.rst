Grains
======

Introduction
------------

Salt includes a built-in mechanism for determining static information about a
system. These bits of information are referred to as *grains* within the Salt
vocabulary. You might think of it as "little *grains* of information" about a
machine. These *grains* of information include hardware and networking
information, operating system details, and much more. These *grains* are also
expandable to include other bits of static information that you'd like to have
assigned to a machine. In this chapter we'll explore the *grains* subsystem and
learn how to leverage this data within our Salt infrastructure.

Goals
-----

Once you've completed this chapter you should have an improved understanding of
what grains are, how grains are queried, defined and synced. You should also understand the
order of grain definition precedence.

.. note::

    If you're not yet familiar with the *grains* system it is important to
    study this entire chapter. Upcoming chapters will make extensive use of
    the information outlined here.

Standard Grains
---------------

Salt includes a set of "core grains" that should be available on any system.
These grains should be detected properly on every supported operating system
and distribution. In this section I'll outline many of these core grains.

The list below defines the full set of core grains found on a CentOS Linux
system. As you can see, these are just the keys without values, but it gives
you an idea of what type of information is stored in grains.

 - SSDs
 - biosreleasedate
 - biosversion
 - cpu_flags
 - cpu_model
 - cpuarch
 - domain
 - fqdn
 - fqdn_ip4
 - fqdn_ip6
 - gpus
 - host
 - hwaddr_interfaces
 - id
 - ip4_interfaces
 - ip6_interfaces
 - ip_interfaces
 - ipv4
 - ipv6
 - kernel
 - kernelrelease
 - locale_info
 - localhost
 - lsb_distrib_codename
 - lsb_distrib_id
 - lsb_distrib_release
 - machine_id
 - manufacturer
 - master
 - mem_total
 - nodename
 - num_cpus
 - num_gpus
 - os
 - os_family
 - osarch
 - oscodename
 - osfinger
 - osfullname
 - osmajorrelease
 - osrelease
 - osrelease_info
 - path
 - productname
 - ps
 - pythonexecutable
 - pythonpath
 - pythonversion
 - saltpath
 - saltversion
 - saltversioninfo
 - selinux
 - serialnumber
 - server_id
 - shell
 - virtual
 - zmqversion

As you can see, there are over fifty items defined for each system within your
Salt infrastructure. These items will be used in upcoming chapters to
demonstrate the flexibility of making your minion management more dynamic.

Let's look at what some of these values store.

Listing Grains
--------------

As you saw in the previous examples, there are a few different ways to query
for grains. Primarily you'll use the ``grains`` module and query for one, all
or specific grain values. If you know the name of the grain you're looking for
you can query for that directly:

.. code-block:: bash

    [root@minion ~]# salt '*' grains.item fqdn
    alpha:
    ----------
    fqdn:
        alpha.domain.tld


.. code-block:: bash

    [root@minion ~]# salt '*' grains.item kernelrelease
    alpha:
    ----------
    kernelrelease:
        2.6.32-431.29.2.el6.x86_64


.. code-block:: bash

    [root@minion ~]# salt '*' grains.item mem_total
    alpha:
    ----------
    mem_total:
        7872

.. code-block:: bash

    [root@minion ~]# salt '*' grains.item cpuarch
    alpha:
    ----------
    cpuarch:
        x86_64

Defining Grains
---------------

Beyond the "core grains" that are defined on every system, it is possible to
define custom grains. These custom grains can be used to define additional
attributes about your systems. Examples of this might be *datacenter*, *rack*,
*cabinet*, or other internal or deployment-specific information. Grains are
defined on a per-minion basis and append to the existing grains.

.. seealso::

    For more information about custom grains replacing existing grains, see the
    next section **Precedence**.

Custom grains can be defined in a couple of places. Again, because grains are
unique per minion, custom grains are defined on a per-minion basis in one of
two places:

  - /etc/salt/minion
  - /etc/salt/grains

There is a slight difference in the way custom grains are imported depending on
the location. First we'll outline the ``/etc/salt/minion`` method, followed by
``/etc/salt/grains``.

**/etc/salt/minion**

Custom grains can be added directly to the minion config file, or included as a
new file in the ``/etc/salt/minion.d/`` directory. If custom grains are added
to either of these locations the whole structure needs to be prefixed with the
``grains`` configuration option. See the example below:

.. code-block:: yaml

    grains:
      datacenter: va5
      rack: 17
      cabinet: 3
      role: webserver

As you can see, custom grains are the same simple key-value pairs that the core
grains are. These can be any arbitrary key-value pair that you want to define
for your systems. In addition to the key-value pairs, you can define other more
complex data structures such as lists or dictionaries. See the example below
for more complex custom grains.

.. code-block:: yaml

    grains:
      role:
        - webserver
        - memcache
      owners:
        - tuttle
        - ewoolley

**/etc/salt/grains**



.. code-block:: yaml

    role:
      - webserver
      - memcache

Precedence
----------

Examples
--------

Syncing & Updating
------------------

