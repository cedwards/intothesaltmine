Writing Modules
===============

While not always required, sometimes it is necessary to write and distribute
custom Salt modules for added functionality or integration with internal
products. This chapter will outline the basic structure and best practices for
creating custom execution modules.

Anatomy of a Salt module
------------------------

When developing custom Salt execution modules there are a few basic rules that
need to be followed. This chapter aims to outline the basic structure of a
module, its key components and general best practices. Let's dive right in.

header
------

To begin I'll outline the first lines of a Salt module. I'll provide an
example, and then explain each section.

.. code-block:: python

    # -*- coding: utf-8 -*-*
    '''
    :maintainer: Christer Edwards (christer.edwards@gmail.com)
    :maturity: 20150907
    :requires: none
    :platform: all
    '''

If you've written Python scripts in the past you might wonder where the
"shebang" is; the `#!/usr/bin/env python` declaration. In the case of a Salt
module (or technically a Python module) this is not required as it is not
called directly from the shell. Because this file will be imported by the Salt
loader, as long as it parses properly, it will become available without a
"shebang".

In this case we simply define the character encoding used within this module.
Unless you need to or have a good reason to use an alternate encoding, `utf-8`
is probably preferred.

For more information on this tag see `PEP-0263`_.

.. _`PEP-0263`: https://www.python.org/dev/peps/pep-0263/

Next we document the module. Due to the way that the Salt documentation is
automatically generated, whatever documentation you define within this
top-level docstring will be used in the documentation page. While this is not
*required* it is preferred, especially if you ever hope to have your module
merged in upstream Salt.

I have written some custom modules that include more content within the
top-level docstring than actual code in the module. This amount of
documentation never hurts, and you'll never be accused of not properly
documenting your code!

imports
-------

.. code-block:: python

    from __future__ import absolute_import

    import logging

At this point you can begin importing the Python modules you require. At
minimum you should use the lines in the example above. You'll likely need more,
but this should always be your baseline.

The `absolute_import` function from the `__future__` provides compatibility
between Python2 and Python3. At the time of this writing Salt is still not
fully Python3 compatible, but using the "future" import standard ensures that
custom modules are at least up to par in that regard.

The second component that you want to import is the logging system. This allows
you to easily add debugging output to your module. This can be extremely
helpful during development and testing, and allows end-users to configure log
levels during runtime.

We'll explore examples of implementing logging later, but for now you should
make sure you import the logging module.

GLOBALS
-------

.. code-block:: python

    LOG = logging.getLogger(__name__)

The above `GLOBAL` definition activates the logger and makes it available
throughout your module. In order to leverage the logging `GLOBAL` variable
use the following syntax:

**LOG Example**:

.. code-block:: python

    LOG.info('info output')
    LOG.debug('debug output')
    LOG.error('error output')
    LOG.warning('warning output')
    LOG.critical('critical output')

__virtualname__
---------------

.. code-block:: python

    __virtualname__ = 'custom_module'

The `__virtualname__` variable definition defines a custom name for your module.
If this definition is missing it will default to the name of the module file
itself (minus the `.py`). While not required, this variable definition is
common to most modules, and often simply matches the Python module name itself.

__virtual__()
-------------

.. code-block:: python

    def __virtual__():
        '''
        Determine whether or not to load this module
        '''
        if salt['grains.get']('os:Linux'):
            return __virtualname__

The `__virtual__()` function is a critical component of any Salt execution
module. This function allows you to enter logic to determine whether or not
your module should load on the given platform. You have full access to Salt
components, including `grains`, `pillar`, testing on the availability of
other Salt execution modules, and more.

"private"
---------

.. code-block:: python

    def _private():
        '''
        "Private" function; only callable within this module
        '''
        ret = some_processed_data

        return ret


"public"
--------

.. code-block:: python

    def public():
        '''
        "Public" function; available to Salt, ie; module.public
        '''

Full Example
------------

.. code-block:: python

    # -*- coding: utf-8 -*-*
    '''
    :maintainer: Christer Edwards (christer.edwards@gmail.com)
    :maturity: 20150907
    :requires: none
    :platform: all
    '''
    from __future__ import absolute_import

    import logging

    LOG = logging.getLogger(__name__)

    __virtualname__ = 'custom_module'


    def __virtual__():
        '''
        Determine whether or not to load this module
        '''
        if salt['grains.get']('os:Linux'):
            return __virtualname__


    def _private():
        '''
        "Private" function; only callable within this module
        '''
        ret = some_processed_data()
        return ret


    def public(*args, **kwargs):
        '''
        "Public" function; available to Salt, ie; module.public
        '''
        ret = _private()
        return ret
