frozenlist
==========

.. image:: https://github.com/aio-libs/frozenlist/workflows/CI/badge.svg
   :target: https://github.com/aio-libs/frozenlist/actions
   :alt: GitHub status for master branch

.. image:: https://codecov.io/gh/aio-libs/frozenlist/branch/master/graph/badge.svg
   :target: https://codecov.io/gh/aio-libs/frozenlist
   :alt: codecov.io status for master branch

.. image:: https://img.shields.io/pypi/v/frozenlist.svg?logo=Python&logoColor=white
   :target: https://pypi.org/project/frozenlist
   :alt: frozenlist @ PyPI

.. image:: https://readthedocs.org/projects/frozenlist/badge/?version=latest
   :target: https://frozenlist.aio-libs.org
   :alt: Read The Docs build status badge

.. image:: https://img.shields.io/matrix/aio-libs:matrix.org?label=Discuss%20on%20Matrix%20at%20%23aio-libs%3Amatrix.org&logo=matrix&server_fqdn=matrix.org&style=flat
   :target: https://matrix.to/#/%23aio-libs:matrix.org
   :alt: Matrix Room — #aio-libs:matrix.org

.. image:: https://img.shields.io/matrix/aio-libs-space:matrix.org?label=Discuss%20on%20Matrix%20at%20%23aio-libs-space%3Amatrix.org&logo=matrix&server_fqdn=matrix.org&style=flat
   :target: https://matrix.to/#/%23aio-libs-space:matrix.org
   :alt: Matrix Space — #aio-libs-space:matrix.org

Introduction
------------

``frozenlist.FrozenList`` is a list-like structure which implements
``collections.abc.MutableSequence``. The list is *mutable* until ``FrozenList.freeze``
is called, after which list modifications raise ``RuntimeError``:


>>> from frozenlist import FrozenList
>>> fl = FrozenList([17, 42])
>>> fl.append('spam')
>>> fl.append('Vikings')
>>> fl
<FrozenList(frozen=False, [17, 42, 'spam', 'Vikings'])>
>>> fl.freeze()
>>> fl
<FrozenList(frozen=True, [17, 42, 'spam', 'Vikings'])>
>>> fl.frozen
True
>>> fl.append("Monty")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "frozenlist/_frozenlist.pyx", line 97, in frozenlist._frozenlist.FrozenList.append
    self._check_frozen()
  File "frozenlist/_frozenlist.pyx", line 19, in frozenlist._frozenlist.FrozenList._check_frozen
    raise RuntimeError("Cannot modify frozen list.")
RuntimeError: Cannot modify frozen list.


FrozenList is also hashable, but only when frozen. Otherwise it also throws a RuntimeError:


>>> fl = FrozenList([17, 42, 'spam'])
>>> hash(fl)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "frozenlist/_frozenlist.pyx", line 111, in frozenlist._frozenlist.FrozenList.__hash__
    raise RuntimeError("Cannot hash unfrozen list.")
RuntimeError: Cannot hash unfrozen list.
>>> fl.freeze()
>>> hash(fl)
3713081631934410656
>>> dictionary = {fl: 'Vikings'} # frozen fl can be a dict key
>>> dictionary
{<FrozenList(frozen=True, [1, 2])>: 'Vikings'}


Installation
------------

::

   $ pip install frozenlist


Documentation
-------------

https://frozenlist.aio-libs.org

Communication channels
----------------------

We have a *Matrix Space* `#aio-libs-space:matrix.org
<https://matrix.to/#/%23aio-libs-space:matrix.org>`_ which is
also accessible via Gitter.

License
-------

``frozenlist`` is offered under the Apache 2 license.

Source code
-----------

The project is hosted on GitHub_

Please file an issue in the `bug tracker
<https://github.com/aio-libs/frozenlist/issues>`_ if you have found a bug
or have some suggestions to improve the library.

.. _GitHub: https://github.com/aio-libs/frozenlist
