.. _isaaclab-pip-installation:

Installation using pip
================================

Issac Lab requires Isaac Sim. Install Isaac Sim first, then Isaac Lab.

Installing Isaac Sim
--------------------

From Isaac Sim 4.0 release, it is possible to install Isaac Sim using pip. This approach is experimental and may have
compatibility issues with some Linux distributions. If you encounter any issues, please report them to the
`Isaac Sim Forums <https://docs.omniverse.nvidia.com/isaacsim/latest/common/feedback.html>`_.

.. attention::

   Installing Isaac Sim with pip requires GLIBC 2.34+ version compatibility.
   To check the GLIBC version on your system, use command ``ldd --version``.

   This may pose compatibility issues with some Linux distributions. For instance, Ubuntu 20.04 LTS has GLIBC 2.31
   by default. If you encounter compatibility issues, we recommend following the
   :ref:`Isaac Sim Binaries Installation <isaaclab-binaries-installation>` approach.

.. attention::

   On Windows with CUDA 12, the GPU driver version 552.86 is required.

.. note::

   If you use Conda, we recommend using `Miniconda <https://docs.anaconda.com/miniconda/miniconda-other-installer-links/>`_.

-  To use the pip installation approach for Isaac Sim, we recommend first creating a virtual environment.
   Ensure that the python version of the virtual environment is **Python 3.10**.

   .. tab-set::

      .. tab-item:: conda environment

         .. code-block:: bash

            conda create -n isaaclab python=3.10
            conda activate isaaclab

      .. tab-item:: venv environment

         .. tab-set::
            :sync-group: os

            .. tab-item:: :icon:`fa-brands fa-linux` Linux
               :sync: linux

               .. code-block:: bash

                  # create a virtual environment named isaaclab with python3.10
                  python3.10 -m venv isaaclab
                  # activate the virtual environment
                  source isaaclab/bin/activate

            .. tab-item:: :icon:`fa-brands fa-windows` Windows
               :sync: windows

               .. code-block:: batch

                  # create a virtual environment named isaaclab with python3.10
                  python3.10 -m venv isaaclab
                  # activate the virtual environment
                  isaaclab\Scripts\activate


-  Next, install a CUDA-enabled PyTorch 2.4.0 build based on the CUDA version available on your system. This step is optional for Linux, but required for Windows to ensure a CUDA-compatible version of PyTorch is installed.

   .. tab-set::

      .. tab-item:: CUDA 11

         .. code-block:: bash

            pip install torch==2.4.0 --index-url https://download.pytorch.org/whl/cu118

      .. tab-item:: CUDA 12

         .. code-block:: bash

            pip install torch==2.4.0 --index-url https://download.pytorch.org/whl/cu121

-  Before installing Isaac Sim, ensure the latest pip version is installed. To update pip, run

   .. code-block:: bash

      pip install --upgrade pip


-  Then, install the Isaac Sim packages. There are 2 options: A complete installation, or a minimal installation for running Isaac Lab only.

   -  Complete installation:

      .. code-block:: bash

         pip install isaacsim==4.2.0.2 isaacsim-extscache-physics==4.2.0.2 isaacsim-extscache-kit==4.2.0.2 isaacsim-extscache-kit-sdk==4.2.0.2 --extra-index-url https://pypi.nvidia.com

   -  Minimal set of packages for running Isaac Lab only:

      .. code-block:: bash

         pip install isaacsim-rl isaacsim-replicator isaacsim-extscache-physics isaacsim-extscache-kit-sdk isaacsim-extscache-kit isaacsim-app --extra-index-url https://pypi.nvidia.com

      Note that you cannot run ``isaacsim`` with this.

Verifying the Isaac Sim installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Make sure that your virtual environment is activated (if applicable)


-  Check that the simulator runs as expected:

   .. code:: bash

      # note: you can pass the argument "--help" to see all arguments possible.
      isaacsim

   By default, this will launch an empty mini Kit window.

-  To run with a specific experience file, run:

   .. code:: bash

      # experience files can be absolute path, or relative path searched in isaacsim/apps or omni/apps
      isaacsim omni.isaac.sim.python.kit


.. attention::

   When running Isaac Sim for the first time, all dependent extensions will be pulled from the registry.
   This process can take upwards of 10 minutes and is required on the first run of each experience file.
   Once the extensions are pulled, consecutive runs using the same experience file will use the cached extensions.

.. attention::

   The first run will prompt users to accept the Nvidia Omniverse License Agreement.
   To accept the EULA, reply ``Yes`` when prompted with the below message:

   .. code:: bash

      By installing or using Isaac Sim, I agree to the terms of NVIDIA OMNIVERSE LICENSE AGREEMENT (EULA)
      in https://docs.omniverse.nvidia.com/isaacsim/latest/common/NVIDIA_Omniverse_License_Agreement.html

      Do you accept the EULA? (Yes/No): Yes


If the simulator does not run or crashes while following the above
instructions, it means that something is incorrectly configured. To
debug and troubleshoot, please check Isaac Sim
`documentation <https://docs.omniverse.nvidia.com/dev-guide/latest/linux-troubleshooting.html>`__
and the
`forums <https://docs.omniverse.nvidia.com/isaacsim/latest/isaac_sim_forums.html>`__.



Installing Isaac Lab
--------------------

Cloning Isaac Lab
~~~~~~~~~~~~~~~~~

.. note::

   We recommend making a `fork <https://github.com/isaac-sim/IsaacLab/fork>`_ of the Isaac Lab repository to contribute
   to the project but this is not mandatory to use the framework. If you
   make a fork, please replace ``isaac-sim`` with your username
   in the following instructions.

Clone the Isaac Lab repository into your workspace:

.. tab-set::

   .. tab-item:: SSH

      .. code:: bash

         git clone git@github.com:isaac-sim/IsaacLab.git

   .. tab-item:: HTTPS

      .. code:: bash

         git clone https://github.com/isaac-sim/IsaacLab.git


.. note::
   We provide a helper executable `isaaclab.sh <https://github.com/isaac-sim/IsaacLab/blob/main/isaaclab.sh>`_ that provides
   utilities to manage extensions:

   .. tab-set::
      :sync-group: os

      .. tab-item:: :icon:`fa-brands fa-linux` Linux
         :sync: linux

         .. code:: text

            ./isaaclab.sh --help

            usage: isaaclab.sh [-h] [-i] [-f] [-p] [-s] [-t] [-o] [-v] [-d] [-c] -- Utility to manage Isaac Lab.

            optional arguments:
               -h, --help           Display the help content.
               -i, --install [LIB]  Install the extensions inside Isaac Lab and learning frameworks (rl_games, rsl_rl, sb3, skrl) as extra dependencies. Default is 'all'.
               -f, --format         Run pre-commit to format the code and check lints.
               -p, --python         Run the python executable provided by Isaac Sim or virtual environment (if active).
               -s, --sim            Run the simulator executable (isaac-sim.sh) provided by Isaac Sim.
               -t, --test           Run all python unittest tests.
               -o, --docker         Run the docker container helper script (docker/container.sh).
               -v, --vscode         Generate the VSCode settings file from template.
               -d, --docs           Build the documentation from source using sphinx.
               -c, --conda [NAME]   Create the conda environment for Isaac Lab. Default name is 'isaaclab'.

      .. tab-item:: :icon:`fa-brands fa-windows` Windows
         :sync: windows

         .. code:: text

            isaaclab.bat --help

            usage: isaaclab.bat [-h] [-i] [-f] [-p] [-s] [-v] [-d] [-c] -- Utility to manage Isaac Lab.

            optional arguments:
               -h, --help           Display the help content.
               -i, --install [LIB]  Install the extensions inside Isaac Lab and learning frameworks (rl_games, rsl_rl, sb3, skrl) as extra dependencies. Default is 'all'.
               -f, --format         Run pre-commit to format the code and check lints.
               -p, --python         Run the python executable provided by Isaac Sim or virtual environment (if active).
               -s, --sim            Run the simulator executable (isaac-sim.bat) provided by Isaac Sim.
               -t, --test           Run all python unittest tests.
               -v, --vscode         Generate the VSCode settings file from template.
               -d, --docs           Build the documentation from source using sphinx.
               -c, --conda [NAME]   Create the conda environment for Isaac Lab. Default name is 'isaaclab'.

Installation
~~~~~~~~~~~~

-  Install dependencies using ``apt`` (on Ubuntu):

   .. code:: bash

      sudo apt install cmake build-essential

- Run the install command that iterates over all the extensions in ``source/extensions`` directory and installs them
  using pip (with ``--editable`` flag):

.. tab-set::
   :sync-group: os

   .. tab-item:: :icon:`fa-brands fa-linux` Linux
      :sync: linux

      .. code:: bash

         ./isaaclab.sh --install # or "./isaaclab.sh -i"

   .. tab-item:: :icon:`fa-brands fa-windows` Windows
      :sync: windows

      .. code:: bash

         isaaclab.bat --install :: or "isaaclab.bat -i"

.. note::

   By default, this will install all the learning frameworks. If you want to install only a specific framework, you can
   pass the name of the framework as an argument. For example, to install only the ``rl_games`` framework, you can run

   .. tab-set::
      :sync-group: os

      .. tab-item:: :icon:`fa-brands fa-linux` Linux
         :sync: linux

         .. code:: bash

            ./isaaclab.sh --install rl_games  # or "./isaaclab.sh -i rl_games"

      .. tab-item:: :icon:`fa-brands fa-windows` Windows
         :sync: windows

         .. code:: bash

            isaaclab.bat --install rl_games :: or "isaaclab.bat -i rl_games"

   The valid options are ``rl_games``, ``rsl_rl``, ``sb3``, ``skrl``, ``robomimic``, ``none``.

Verifying the Isaac Lab installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To verify that the installation was successful, run the following command from the
top of the repository:

.. tab-set::
   :sync-group: os

   .. tab-item:: :icon:`fa-brands fa-linux` Linux
      :sync: linux

      .. code:: bash

         # Option 1: Using the isaaclab.sh executable
         # note: this works for both the bundled python and the virtual environment
         ./isaaclab.sh -p source/standalone/tutorials/00_sim/create_empty.py

         # Option 2: Using python in your virtual environment
         python source/standalone/tutorials/00_sim/create_empty.py

   .. tab-item:: :icon:`fa-brands fa-windows` Windows
      :sync: windows

      .. code:: batch

         :: Option 1: Using the isaaclab.bat executable
         :: note: this works for both the bundled python and the virtual environment
         isaaclab.bat -p source\standalone\tutorials\00_sim\create_empty.py

         :: Option 2: Using python in your virtual environment
         python source\standalone\tutorials\00_sim\create_empty.py


The above command should launch the simulator and display a window with a black
viewport as shown below. You can exit the script by pressing ``Ctrl+C`` on your terminal.
On Windows machines, please terminate the process from Command Prompt using
``Ctrl+Break`` or ``Ctrl+fn+B``.


.. figure:: ../../_static/setup/verify_install.jpg
    :align: center
    :figwidth: 100%
    :alt: Simulator with a black window.


If you see this, then the installation was successful! |:tada:|
