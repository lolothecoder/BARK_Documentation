Jetson & ROS 2 Setup
====================

BYTE's onboard computer is an NVIDIA **Jetson Orin Nano**. This page explains how it was set up from scratch: installing JetPack 7.2.1 (Ubuntu 24.04), then Git, then **ROS 2 Jazzy**. Follow it in order if the Jetson ever has to be reinstalled.

.. warning::

   The JetPack installation **erases the target drive**. Back up everything on the Jetson before you start.

----

Why a Fresh Install Was Needed
------------------------------

* A Jetson **cannot** be upgraded from Ubuntu 22.04 to 24.04 in place. Do **not** run ``do-release-upgrade``: NVIDIA advises against it. The board's drivers, kernel and firmware come from JetPack and break if only Ubuntu is upgraded.
* Ubuntu 24.04 on the Orin Nano comes with JetPack 7.2 or newer (L4T R39.x, CUDA 13.2).
* From JetPack 7.2 on, NVIDIA no longer provides ready-made SD card images. The supported way is the **Jetson ISO** installer, written to a USB stick.

----

JetPack 7.2.1 Installation (USB Installer)
------------------------------------------

**What you need:**

* A USB stick of 16 GB or more (it gets erased)
* A DisplayPort monitor and a USB keyboard
* The target drive: an NVMe SSD, fitted in the Jetson **before** you start
* A PC with `Balena Etcher <https://etcher.balena.io/>`_

**Step 1 — Back up the Jetson**

If there are files on the Jetson, copy the home folder, projects, config changes and Docker volumes to another drive.

**Step 2 — Download the installer**

There is one image for all Jetsons: `jetsoninstaller-r39.2.1 (ISO) <https://developer.nvidia.com/downloads/embedded/l4t/r39_release_v2.1/iso/jetsoninstaller-r39.2.1-2026-08-07-18-30-47-arm64.iso>`_. You may need to log in to a free NVIDIA developer account.

**Step 3 — Write it to the USB stick**

In Balena Etcher: choose the ISO, choose the USB stick, then :guilabel:`Flash`.

**Step 4 — Check the Jetson's firmware version**

Power on, keep pressing :kbd:`Esc` at the NVIDIA logo, and read the version at the top of the setup menu. It must be **36.0 or higher** (JetPack 6 firmware) before using the JetPack 7 installer.

**Step 5 — Start the installer from USB**

In the :kbd:`Esc` menu, go to :menuselection:`Boot Manager` and pick the USB disk.

**Step 6 — Install**

1. In the menu, pick **Install Jetson ISO r39.2.1**.
2. **Firmware update:** the installer asks to update the firmware and only waits **30 seconds**. Press :kbd:`Y`. The update runs in two passes and the board may restart by itself.
3. Choose the target drive (NVMe).
4. Confirm that it will be erased, wait until it finishes, and reboot when asked.

.. warning::

   Do not power off the Jetson or unplug the USB stick during the firmware update. If you miss the 30 second prompt, the new system will not boot (see the next section).

**Step 7 — First boot**

Remove the USB stick. Accept the license, then set the language, keyboard, time zone, network and user account.

----

Troubleshooting: Black Screen After Install
-------------------------------------------

**What happened:** after the install and reboot, the screen showed only ``-`` for more than 15 minutes.

**Diagnosis:** pressing :kbd:`Esc` at the NVIDIA logo still showed firmware version 36.x. The firmware update (the :kbd:`Y` prompt of step 6) had not been applied, and the new R39 system cannot start on the old R36 firmware.

**Fix:**

1. Plug the installer USB stick back in and power on.
2. :kbd:`Esc`, then :menuselection:`Boot Manager --> USB disk`.
3. Watch for the firmware update prompt and press :kbd:`Y` within 30 seconds.
4. Don't touch anything while it updates: leave the USB stick in, leave the power on, and let the board restart by itself.
5. Check: :kbd:`Esc` at the logo should now show version 39.x.
6. Reinstall to the same drive if offered, then remove the USB stick and reboot.

**Other causes worth checking for a black screen:**

* **The USB stick is still plugged in**, so the Jetson boots from it again.
* **Display adapter:** a passive DisplayPort-to-HDMI adapter can go blank once Ubuntu starts. Use a direct DisplayPort cable or an *active* adapter. :kbd:`Ctrl+Alt+F2` opens a text console to test.
* **Wrong startup drive:** :kbd:`Esc`, then :menuselection:`Boot Manager`, and pick the drive you installed to.
* **The Y prompt never appears:** as a fallback, use NVIDIA SDK Manager on a PC running Ubuntu 24.04, with the Jetson in recovery mode (jumper across pins 9-10 of the J14 button header at power-on).

----

Post-Install Checks
-------------------

.. code-block:: bash

   lsb_release -a                        # Ubuntu 24.04
   cat /etc/nv_tegra_release             # R39
   sudo apt update && sudo apt full-upgrade -y
   sudo apt install -y nvidia-jetpack    # CUDA 13 etc. if not already there

.. note::

   JetPack 7 uses CUDA 13. Rebuild your own CUDA code and Python wheels, and use Docker images built for L4T R39 / JetPack 7. Images made for JetPack 6 won't match.

----

Git
---

.. code-block:: bash

   sudo apt update
   sudo apt install -y git
   git config --global user.name  "ByteJetson"
   git config --global user.email "bytequadruped@gmail.com"

----

ROS 2 Jazzy Installation
------------------------

ROS 2 Jazzy is built for Ubuntu 24.04 and has official arm64 packages. Run all the following commands in a terminal on the Jetson.

**Step 1 — Language settings (UTF-8)**

Check that the Jetson uses a UTF-8 locale:

.. code-block:: bash

   locale

**Step 2 — Turn on the "universe" package source**

.. code-block:: bash

   sudo apt install -y software-properties-common
   sudo add-apt-repository -y universe

**Step 3 — Add the ROS 2 package source**

.. code-block:: bash

   sudo apt update && sudo apt install -y curl
   export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
   curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
   sudo dpkg -i /tmp/ros2-apt-source.deb

The version installed on BYTE's Jetson is ``ros2-apt-source 1.3.0``.

**Step 4 — Update the system first (don't skip this)**

.. code-block:: bash

   sudo apt update
   sudo apt upgrade -y

**Step 5 — Install ROS 2 Jazzy and the developer tools**

.. code-block:: bash

   sudo apt install -y ros-jazzy-desktop ros-dev-tools

``desktop`` includes the graphical tools RViz and rqt.

**Step 6 — Load ROS automatically in every new terminal**

.. code-block:: bash

   echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
   source ~/.bashrc

**Step 7 — Set up rosdep**

``rosdep`` installs the packages that ROS projects need.

.. code-block:: bash

   sudo rosdep init
   rosdep update

----

Testing ROS 2 (Talker / Listener)
---------------------------------

Open two terminals:

.. code-block:: bash

   # Terminal 1
   ros2 run demo_nodes_cpp talker

   # Terminal 2
   ros2 run demo_nodes_py listener

It works if terminal 2 prints ``I heard: [Hello World: N]``. This test passed on BYTE's Jetson.

To stop, press :kbd:`Ctrl+C` in each terminal. If something won't stop:

.. code-block:: bash

   pkill -f demo_nodes

----

BYTE's Accounts
---------------

The Jetson has its own accounts, so that nothing on it depends on a member's personal account.

* **Gmail:** ``bytequadruped@gmail.com``
* **GitHub:** username ``ByteJetson``, registered with the Gmail address above

.. note::

   The passwords are **not** written here. They are stored in the AI Team password manager.

The Jetson has its own branch, ``ByteJetson``, on the `BARK repository <https://github.com/EPFL-AI-Team/BARK/tree/ByteJetson>`_. It pushes there whenever the codebase is changed directly on the Jetson.

----

Sources
-------

* `NVIDIA JetPack 7.2.1 downloads and notes <https://developer.nvidia.com/embedded/jetpack/downloads>`_
* `Orin Nano Quick Start Guide <https://docs.nvidia.com/jetson/orin-nano-devkit/user-guide/latest/quick_start.html>`_
* `ROS 2 Jazzy Ubuntu install guide <https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html>`_
