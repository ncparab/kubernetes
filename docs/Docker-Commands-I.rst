####################
Docker Commands - I
####################


Followings are widely used docker commands
-------------------------------------------

1) docker search
==================

Search the Docker Hub for images
Example:

.. code-block:: bash

    $ docker search fedora

.. image:: d1.png
   :width: 600px
   :height: 400px
   :alt: alternate text
   
   
2) docker pull
===============

The docker pull command serves for downloading Docker images from a registry.

By default, the docker pull command pulls images from Docker Hub, but it is also possible to manually specify the private registry to pull
from.
Before running the docker pull command it needs to search the Docker registry for the image to download.

.. code-block:: bash

    $ docker pull fedora
    
.. image:: d2.png
   :width: 600px
   :height: 400px
   :alt: alternate text
