.. _devenv:

.. sectionauthor:: Bernhard Posselt <dev@bernhard-posselt.com>

=======================
Development Environment
=======================

Please follow the steps on this page to set up your development environment.

Set up Web server and database
==============================

First `set up your Web server and database <https://docs.nextcloud.org/server/11/admin_manual/installation/index.html>`_ (**Section**: Manual Installation - Prerequisites).

.. TODO ON RELEASE: Update version number above on release

Get the source
==============

There are two ways to obtain Nextcloud sources:

* Using the `stable version <https://docs.nextcloud.org/server/11/admin_manual/#installation>`_
.. TODO ON RELEASE: Update version number above on release
* Using the development version from `GitHub`_ which will be explained below.

To check out the source from `GitHub`_ you will need to install git (see `Setting up git <https://help.github.com/articles/set-up-git>`_ from the GitHub help)

Gather information about server setup
-------------------------------------

To get started the basic git repositories need to cloned into the Web server's directory. Depending on the distribution this will either be

* **/var/www**
* **/var/www/html**
* **/srv/http**


Then identify the user and group the Web server is running as and the Apache user and group for the **chown** command will either be

* **http**
* **www-data**
* **apache**
* **wwwrun**

Check out the code
------------------

The following commands are using **/var/www** as the Web server's directory and **www-data** as user name and group.

After the development tool installation make the directory writable::

  sudo chmod o+rw /var/www

Then install Nextcloud from git::

  git clone git@github.com:nextcloud/server.git /var/www/<folder>
  cd /var/www/<folder> 
  git submodule update --init

where <folder> is the folder where you want to install Nextcloud.

Adjust rights::

  sudo chown -R www-data:www-data /var/www/core/data/
  sudo chmod o-rw /var/www


Finally restart the Web server (this might vary depending on your distribution)::

  sudo systemctl restart httpd.service

or::

  sudo /etc/init.d/apache2 restart

After the clone Open http://localhost/core (or the corresponding URL) in your web browser to set up your instance.

Enabling debug mode
-------------------
.. _debugmode:

.. note:: Do not enable this for production! This can create security problems and is only meant for debugging and development!

To disable JavaScript and CSS caching debugging has to be enabled by setting ``debug`` to ``true`` in :file:`core/config/config.php`::

  <?php
  $CONFIG = array (
      'debug' => true,
      ... configuration goes here ...
  );

Keep the code up-to-date
------------------------

If you have more than one repository cloned, it can be time consuming to do the same the action to all repositories one by one. To solve this, you can use the following command template::

  find . -maxdepth <DEPTH> -type d -name .git -exec sh -c 'cd "{}"/../ && pwd && <GIT COMMAND>' \;

then, e.g. to pull all changes in all repositories, you only need this::

  find . -maxdepth 3 -type d -name .git -exec sh -c 'cd "{}"/../ && pwd && git pull --rebase' \;

or to prune all merged branches, you would execute this::

  find . -maxdepth 3 -type d -name .git -exec sh -c 'cd "{}"/../ && pwd && git remote prune origin' \;

It is even easier if you create alias from these commands in case you want to avoid retyping those each time you need them.


.. _GitHub: https://github.com/nextcloud
.. _GitHub Help Page: https://help.github.com/
