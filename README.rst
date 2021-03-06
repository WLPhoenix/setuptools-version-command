setuptools-version-command 
==========================

*Get version from version control instead of hardcoding it into setup.py*

introduction
------------

Instead of hardcoding the version in setup.py like this:

.. code-block:: python

    setup(
        name='some-dist-name',
        version='1.0',
        ...)

This package allows specifying a VCS command like this:

.. code-block:: python

    setup(
        name='some-dist-name',
        version_command='git describe',
        ...)

...Which will then update the version into some-dist-name.egg-info/version.txt,
so that the version can also be found if setup.py is being run from a sdist
or something.

When it can find the version from either some-dist-name.egg-info/version.txt
or the output of the version_command, it will update the version key that is
normally used for the version, such as what's used in the sdist filename and
so on.

setup keyword
-------------

Instead of specifying the ``version`` keyword argument, specify the ``version_command``
keyword argument. It can either be a str or a tuple. If it's a str, it's interpreted
as just the command to execute, for example ``git describe``. If it's a tuple, it must
have two elements, the first must be the command, and the second specifies how to
adapt the version string to PEP440 and must one have one of the following values:

``None`` takes output of command as is:

    Do nothing, ignore PEP440 and accept that pip/setuptools will throw warnings

``pep440-git-local`` moves number of commits and hash as local identifier:

    * Change ``1.2.3`` to ``1.2.3``
    * Change ``1.2.3-10-abc1234`` to ``1.2.3+git-10-abc1234``

``pep440-git-dev`` or ``pep440-git`` moves number of commits into development release identifier and omits hash:

    * Change ``1.2.3.dev`` to ``1.2.3.dev0``
    * Change ``1.2.3.dev-N-abc1234`` to ``1.2.3.devN``
    * Change ``1.2.3`` to ``1.2.3``

    In other works, it excepts you to create a separate ``.dev``-ending tag after each release, until you create a
    release tag again. If you are not on tag, and don't have ``.dev``-ending tag, following will happen:

    * Change ``1.2.3-N-abc1234`` to ``1.2.3.devN``

    This is against PEP440-specification and supported only for backwards-compatibility. It may be removed in time.

installation
------------

To use it, just do this::

    pip install setuptools-version-command

With the pip of the global python. (Or use ``--user``.)

testing
-------

To test it, run ``./setup.py --version``.

developing
----------

Within a checkout of this repo:

.. code-block:: shell

    virtualenv env
    env/bin/pip install --editable .
    env/bin/python setup.py --version

Make sure that you change the ``setup.py`` so that it actually makes use of setuptools-version-command.
