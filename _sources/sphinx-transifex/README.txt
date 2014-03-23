=====================================
txutil: transifex utility for Sphinx
=====================================

`txutil` is a Transifex utility tool that provides several features that make it easy to use the Transifex service for translation with Sphinx.

BTW, this tool is focused on transifex and is dependent on transifex_client external library and polib external library.

Features
=========

* create .transifexrc file from environment variable, without interactive input.
* create .tx/config file without interactive input.
* update .tx/config file from locale/pot files automatically.
* build mo files from po files in the locale directory.

You need to use `tx` command for below features:

* `tx push -s` : push pot (translation catalogs) to transifex.
* `tx pull -l ja` : pull po (translated catalogs) from transifex.


Requirements
=============

- Python 2.5, 2.6, 2.7. (depends transifex_client that only support 2.x)

- pip_ 1.2 or later.

- Your transifex_ account if you want to download po files from transifex
  or you want to translate on transifex.

.. _transifex: https://transifex.com
.. _pip: https://pypi.python.org/pypi/pip


Installation
=============

Recommend strongly: use virtualenv for this procedure.

::

   $ pip install https://bitbucket.org/shimizukawa/sphinx-transifex/get/default.zip


Commands, options, environment variables
=========================================

Type `txutil` without arguments, options.


Setup environment for `tx` command
===================================

Set environments::

   $ export TXUTIL_TRANSIFEX_USERNAME=<YOUR TRANSIFEX ACCOUNT USERNAME>
   $ export TXUTIL_TRANSIFEX_PASSWORD=<YOUR TRANSIFEX ACCOUNT PASSWORD>

Create ``~/.transifexrc`` file for transifex_ account settings::

   $ txutil create-transifexrc

Create ``.tx/config`` for transifex project setings::

   $ txutil create-txconfig

Add below settings to sphinx document's conf.py if not exists::

   locale_dirs = ['locale/']   #for example
   gettext_compact = False     #optional

Build document's pot files and auto setup transifex resource config::

   $ make gettext
   $ mkdir locale
   $ cp -R _build/locale locale/pot
   $ txutil update-txconfig-resources --locale-dirs=locale

Done. You got .tx/config for the document translation project.


Make new translation team for new language
===========================================

1. login to transifex_ service.
2. go to `sphinx translation page`_.
3. push ``Request language`` and fill form.
4. wait acceptance by transifex sphinx translation maintainers.
5. (after acceptance) translate on transifex.

.. _`sphinx translation page`: https://www.transifex.com/projects/p/sphinx-doc-1_2_0/ 


Make translated document
=========================

Get translated catalogs and build mo files (ex. for 'ja')::

   $ tx pull -l ja
   $ txutil --locale-dirs=locale build-mo


Build html (ex. for 'ja')::

   $ make -e SPHINXOPTS="-D language='ja'" html

Done.



Forward the translation
========================

1. login to transifex_ service.
2. go to `sphinx translation page`_.
3. click language name (ex. 'Japanese') you want to translate.
4. click document name (ex. 'domains') you want to translate.
5. click start. The document will be locked in order to avoid conflicts.
6. click save-exit. Please don't leave the page without save-exit.
7. go to 4.


Upload local translated po files to transifex
==============================================

If you want to push all language's po files, you can use `tx push -t`.
(this operatoin overwrite translations in transifex.)

::

   $ tx push -t -l ja


Upload pot files to transifex server
=====================================

If pot files are updated, you need to push your new pot files to transifex.

::

   $ make gettext
   $ mkdir locale
   $ cp -R _build/locale locale/pot
   $ txutil update-txconfig-resources --locale-dirs=locale
   $ tx push -s

After this operation, you can get merged po files by using `tx pull -l ja` command.

