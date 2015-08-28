Writing Modules
===============

While not always required, sometimes it is necessary to write and distribute
custom Salt modules for added functionality or integration with internal
products. This chapter will outline the basic structure and best practices for
creating custom execution modules.

Anatomy of a Salt module
------------------------

When developing custom Salt excution modules there are a few basic rules that
need to be followed. This chapter aims to outline the basic structure of a
module, its key components and general best practices. Let's dive right in.

header
------

To begin I'll outline the first five lines of a Salt module. I'll provide an
example, and then explain each section.

.. code-block:: python

    # -*- coding: utf-8 -*-*
    '''
    My custom Salt module
    '''

If you've written Python scripts in the past you might wonder where the
"shebang" is; the '#!/usr/bin/env python` declaration. In the case of a Salt
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


__virtual__()
-------------
    
    def __virtual__():
        '''
        Determine whether or not to load this module
        '''
        return True

"private"
---------


    def _private():
        '''
        "Private" function; only callable within this module
        '''
        ret = some_processed_data

        return ret


"public"
--------

    def public():
        '''
        "Public" function; available to Salt, ie; module.public
        '''
