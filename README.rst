RealtimePi
==========

An out of the box `Raspberry Pi <http://www.raspberrypi.org/>`_ Raspbian distro with a realtime kernel. The kernel is a  `RaspberryPi kernel <https://github.com/raspberrypi/linux>`_ patched with the `Preempt RT patch <https://wiki.linuxfoundation.org/realtime/start>`_. Kernel patches are `taken from here <https://wiki.linuxfoundation.org/realtime/documentation/howto/applications/preemptrt_setup>`_.

RealtimePi uses `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_, and you can use RealtimePi as a base image for CustomPiOS to make your own realtime kernel-based disros.

Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/RealtimePi>`_

Nightly builds are available `here <http://unofficialpi.org/Distros/RealtimePi/nightly/>`_ (currently built on demand)

How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``alarmpi-wpa-supplicant.txt`` or ``realtimepi-network.txt`` on the root of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card


Requirements
------------
* Raspberrypi 1/zero and newser.
* 2A power supply

Features
--------

* Realtime kernel out of the box for both armv6 and armv7. All Pies that Raspbian supports are supported.
* Tools to configure the network via text files /boot

Developing
----------

Requirements
~~~~~~~~~~~~

#. `qemu-arm-static <http://packages.debian.org/sid/qemu-user-static>`_
#. `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_
#. Downloaded `Raspbian <http://www.raspbian.org/>`_ image.
#. root privileges for chroot
#. Bash
#. realpath
#. sudo (the script itself calls it, running as root without sudo won't work)

Build RealtimePi From within Raspbian / Debian / Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

RealtimePi can be built from Debian, Ubuntu, Raspbian.
Build requires about 3.5 GB of free space available.
Note it takes about 6 hours to builds the kernel, and 12 for both kernels!
You can build it by issuing the following commands::

    sudo apt-get install realpath p7zip-full qemu-user-static
    
    git clone https://github.com/guysoft/CustomPiOS.git
    git clone https://github.com/guysoft/RealtimePi.git
    cd RealtimePi/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspbian_lite_latest'
    cd ..
    ../../CustomPiOS/src/update-custompios-paths
    sudo modprobe loop
    sudo bash -x ./build_dist
    
Building RealtimePi Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

RealtimePi supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo bash -x ./build_dist [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build RealtimePi in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd RealtimePi/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd RealtimePi/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd RealtimePi/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building RealtimePi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
