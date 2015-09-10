Writing Modules
===============

While not always required, sometimes it is necessary to write and distribute
custom Salt modules for added functionality or integration with other
products. This chapter will outline the basic structure and best practices for
creating custom execution modules.

Anatomy of a Salt module
------------------------

When developing custom Salt execution modules there are a few basic rules that
need to be followed. This chapter aims to outline the basic structure of a
module, its key components and general best practices. Let's dive right in.

header
------

.. code-block:: python

    # -*- coding: utf-8 -*-
    '''
    :maintainer: Christer Edwards (christer.edwards@gmail.com)
    :maturity: 20150910
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

The above `GLOBAL` activates the logger and makes it available
throughout your module. In order to leverage the logging `GLOBAL`
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

This definition also allows the module layer to be abstracted, and is what allows
a standard command across multiple platforms. For example, the `pkg` module supports
a wide range of binary package providers. From `yum` to `apt-get` and everywhere
in-between. Each of these defines `__virtualname__` as `pkg`, meaning based on the
conditional statements within the `__virtual__` function only the appropriate `pkg` 
provider is loaded.

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

Functions
=========

A Salt execution module generally consists of "private" and "public" functions.
These functions are either callable from within the module itself (private), or
callable directly through Salt (public). The way Salt treats functions within
these custom modules very much follows the Pythonic way of handling modules
and methods.

In this section I provide examples of both types of functions:

"private"
---------

.. code-block:: python

    def _private():
        '''
        "Private" function; only callable within this module
        '''
        LOG.debug('Executing the _private function')
        
        ret = {}
        return ret

A "private" function works the same way that any other function does. The only
difference here is that the function name is preceded with an underscore (`_`).
Any function prefixed with an underscore character will only be
available within the module, and will not be directly callable through Salt.

"public"
--------

.. code-block:: python

    def public(*args, **kwargs):
        '''
        "Public" function; available to Salt, ie; module.public
        
        CLI Example:
        
            salt '*' module.public
        '''
        LOG.debug('Executing the public function')
        
        ret = _private()
        return ret

"public" functions within an execution module are mapped and made available to
the Salt administrators. Any "public" function becomes available to be called
from Salt, prefixed by the module name. For example, if our custom module was
called "module", and our function name was "public", we'd call this through Salt
by targeting `module.public`.

Full Example
------------

.. code-block:: python

    # -*- coding: utf-8 -*-
    '''
    :maintainer: Christer Edwards (christer.edwards@gmail.com)
    :maturity: 20150910
    :requires: none
    :platform: all
    '''
    from __future__ import absolute_import

    import logging

    LOG = logging.getLogger(__name__)

    __virtualname__ = 'module_name'


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
        LOG.debug('Executing the _private function')
        
        ret = {}
        return ret


    def public(*args, **kwargs):
        '''
        "Public" function; available to Salt, ie; module.public
        
        CLI Example:
        
            salt '*' module.public
        '''
        LOG.debug('Executing the public function')
        
        ret = _private()
        return ret

Running Commands & Executing Modules
====================================

Often times a custom execution module is simply a wrapper around a command line
utility. This means that "under the hood" Salt is simply executing an existing
command with certain options. When you realize how this works your first
thought in regards to development might be "Perfect. So I'll use `subprocess`
and call the binary..." While that may be the right approach in other cases,
Salt makes this simpler. Salt makes all other loaded modules available to your
custom module. This means you can call any other available Salt module through
your Salt module, including `cmd` to run arbitrary commands. Please do not
use `subprocess` in your custom module unless you have a very good reason to do
so. Use the existing `cmd` module to execute arbitrary commands. An example
might be as follows:

.. code-block:: python

    cmd = '{0} {1} {2}'.format('egrep', string, filename)
    ret = salt['cmd.run'](cmd)


This function does not process commands through a shell unless the `python_shell`
flag is set to `True`. This means that any shell-specific functionality such as
'echo' or the use of pipes, redirection or &&, should either be migrated to
`cmd.shell` or have the `python_shell=True` flag set here.

.. note:: Security Notice

    The use of python_shell=True means that the shell will accept _any_ input
    including potentially malicious commands such as 'good_command; rm -rf /'. Be
    absolutely certain that you have sanitized your input prior to using
    python_shell=True

