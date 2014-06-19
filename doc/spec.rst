======
Helori
======

---------------------------------------------
Archlinux repositories hosting and management
---------------------------------------------

Goal
====

Helori's goal is to simplify custom Archlinux repository hosting and
management. It targets small business usage.

Let's assume that in my company we have 4 repositories:
 - third-parties, which contains custom made packages for third-parties or
   built packages from AUR
 - dev, developpment environment
 - test, test environment
 - prod, prod environmnet

We want to easily:
 - upload packages to a pool of packages
 - select which packages goes from the pool to a repository
 - keep every version of every packages
 - easy to rollback
 - compare packages and versions in dev, test and prod

Features
========

 - user management

   * external authentification

 - package pool

   * client upload tool
   * HTTP access to the pool (HEAD, GET)
   * upload ACL, note that sensible file like certificate or password
     configuration files should not be in the packages
   * no overwrite
   * no remove

 - repositories

   * http exposure of the repositories
   * add package
   * remove package

 - HTTP API

   * list package pool content
   * list repository content
   * compare repository content
   * basic operations on repo (add, remove package)

Overview
========

.. image:: overview.svg

HTTP Routing
============

 - /pool/ access to the archives pool
 - /repos/$repo_name/ access to the repository
 - /api/ API mapping

HTTP API
========

Authentication
~~~~~~~~~~~~~~

The authentication is done through HTTP headers:
::

  X-Helori-User: abique
  X-Helori-Password: WfAEaXmcrF4Hck3TpntJnX4

Package upload
~~~~~~~~~~~~~~

::

  PUT /pool/wine-stable-1.6.3-1.tar.pkg.x86_64.pkg.tar.xz HTTP/1.0
  X-Helori-User: abique
  X-Helori-Password: WfAEaXmcrF4Hck3TpntJnX4
  ...

List pool packages
~~~~~~~~~~~~~~~~~~

::
   GET /api/pool

::
   [{
     "package": "wine-stable",
     "version": "1.6.3",
     "pkgver": "1"
   }, ...]

Interfaces list
~~~~~~~~~~~~~~~

Configuration
=============
