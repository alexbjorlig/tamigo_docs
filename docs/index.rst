.. Tamigo webservices documentation master file, created by
   sphinx-quickstart on Sat Jun 18 12:05:07 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to Tamigo webservices's documentation!
==============================================
Tamigo offers a set of REST based services for you to create new ways to interact with Tamigo. The services are divided into two categories.
The first set of services can be called using an application access, and is meant to be used for integrating third party applications with Tamigo
The other set of services uses a Tamigo user as authentication, and will return data based on the users context. These services can be used to create other user experiences for Tamigo, and we use it ourselves to power the mobile applications for Tamigo.
The services are all capable of returning xml or json-formatted data. Which response you get will be based on type of input and the specified content-type in the HTTP headers.
Services URL: https://services.tamigo.com can also be used without SSL.


Contents:

.. toctree::
   :maxdepth: 2
   :hidden:

   application_services




Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

