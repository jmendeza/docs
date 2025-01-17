:is-up-to-date: True

:orphan:

.. document does not appear in any toctree, and is only accessible via searching.
   use :orphan: File-wide metadata option to get rid of WARNING: document isn't included in any toctree for now

.. index:: Installing CrafterCMS on WSL 2

.. _installing-craftercms-on-wsl:

==============================
Installing CrafterCMS on WSL 2
==============================

This section describes in detail how to install/setup CrafterCMS on Windows via WSL 2.

First, we need to install Windows Subsystem for Linux (WSL) by following the instructions
`here <https://docs.microsoft.com/en-us/windows/wsl/install>`__ which will enable the required
optional components, download the latest Linux kernel, set WSL 2 as your default, and install
Ubuntu Linux for you.

All the commands/scripts in this section should be executed in your Ubuntu (or Linux distribution of choice) terminal.

To open your Ubuntu terminal, click on the Ubuntu icon from the Start menu, or, from a
command prompt, type ``ubuntu`` or ``ubuntu1604`` or ``ubuntu2004`` depending on the version
you've downloaded.

.. figure:: /_static/images/system-admin/open-ubuntu-terminal.jpg
   :alt: Setting up CrafterCMS in Windows - Open the Ubuntu terminal
   :width: 70 %
   :align: center

|


It is recommended we store all files in the WSL file system for better performance speed.

See https://docs.microsoft.com/en-us/windows/wsl/ for more information on WSL.

.. _installing-crafter-cms-on-wsl:

------------------------------------------------
Installing CrafterCMS from the Prebuilt Binaries
------------------------------------------------

Here are the steps to start using CrafterCMS for development or evaluation by installing CrafterCMS from the prebuilt binaries:

#. **Download and install Java 11**

   Download and install Java JDK 11 (either `Oracle <http://www.oracle.com/technetwork/java/javase/downloads/index.html>`_  or `OpenJDK <http://openjdk.java.net/>`_).

   Make sure that you have a ``JAVA_HOME`` environment variable that points to the root of the JDK install directory.  See :ref:`here<verify-java-home-env-var>` for more information on the ``JAVA_HOME`` environment variable

   Here's an example of installing Java JDK 11 using ``apt`` then setup ``JAVA_HOME``

   .. code-block:: bash
      :caption: *Install Java JDK 11 and setup JAVA_HOME*

      sudo apt install openjdk-11-jdk
      export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
      export PATH=$PATH:$JAVA_HOME/bin

   |

#. **Download CrafterCMS binaries**

   Download the CrafterCMS install prebuilt binaries from https://craftercms.org/downloads

   Select ``crafter-cms-authoring-VERSION.tar.gz``.  The ``.tar.gz`` file will install a fully functional authoring instance. Out of the box, the authoring instance uses a local directory as the repository and an embedded database, which allows a quick and easy set up for local development.

   You can download the CrafterCMS prebuilt binaries directly onto the WSL file system from the Ubuntu terminal using ``wget`` or ``curl``, or, you can copy/move the prebuilt binaries in the Windows file system to the WSL file system via the Ubuntu terminal or the Windows File Explorer.

   The Linux (WSL) file system root directory is : ``\\wsl$\Ubuntu-20.04\home\<user name>\path\to\project``

   The Windows file system root directory is : ``/mnt/c/Users/<user name>/path/to/project$`` or ``C:\Users\<user name>\path\to\project``

   .. figure:: /_static/images/system-admin/accessing-wsl-fs-in-explorer.png
      :alt: Setting up CrafterCMS in Windows - Accessing the WSL file system
      :width: 70 %
      :align: center

   |

#. **Extract the CrafterCMS binaries**

   Extract the contents in any directory.

   .. code-block:: sh
      :caption: *Extract the contents of the CrafterCMS binary archive file to a directory*

      tar -zxvf crafter-cms-authoring-VERSION.tar.gz -C /tmp/extract_to_some_directory/

   |

   The extracted files should look like this:

   .. code-block:: none
      :caption: *CrafterCMS extracted files directory structure*

      {Crafter-CMS-unzip-directory}
      |--crafter/
         |--LICENSE
         |--README.txt
         |--bin/
         |--data/
            |--ssh/
               |--config
               |--known-hosts

   |

#. **Start CrafterCMS**

   **To start CrafterCMS:**

   From the command line, navigate to the ``{Crafter-CMS-unzip-directory}/crafter/bin/`` directory, and execute the startup script:

   .. code-block:: sh
      :caption: *Start CrafterCMS*

      ./startup.sh

   |

      .. note::

         *It takes a few seconds for CrafterCMS to startup and takes longer to startup the very first time you startup CrafterCMS.*

   |

   .. figure:: /_static/images/system-admin/start-crafter-in-wsl2.png
      :alt: Setting up CrafterCMS in Windows - Start CrafterCMS in WSL
      :width: 70 %
      :align: center

   |


   **To stop CrafterCMS:**

   From the command line, navigate to the ``{Crafter-CMS-unzip-directory}/crafter/bin/`` directory, and execute the shutdown script:

   .. code-block:: sh
      :caption: *Stop CrafterCMS*

      ./shutdown.sh

   |



#. **Access Crafter Studio**

   In your browser, go to

   .. code-block:: none

      http://localhost:8080/studio

   |

   * Login with the following:

      * **username:** admin
      * **password:** admin


   After logging in, you should be redirected to the ``Sites`` screen, and you're now ready to create your first experience!
