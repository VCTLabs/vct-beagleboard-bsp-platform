VCT Beagleboard BSP startup
===========================

To get the BSP you need to have repo installed and use it as:

Install the repo utility
------------------------

::

  $ mkdir ~/bin
  $ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  $ chmod a+x ~/bin/repo

Download the BSP source
-----------------------

::

  $ PATH=${PATH}:~/bin
  $ mkdir beagleboard-bsp
  $ cd beagleboard-bsp
  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b oe-fido
  $ repo sync

At the end of the commands you have every metadata you need to start work with.

To start a simple image build::

  $ cd oe-core
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to beaglebone
  $ bitbake core-image-minimal

You can use any directory (build-dir above) to host your build.  The above commands will
build an image for beaglebone using the oe-core and either meta-ti BSP machine config or
meta-beagleboard BSP (plus meta-ti deps).  Edit bblayers.conf to select the BSP dir,
either meta-ti or meta-beagleboard/common-bsp.  If you have unbuildable packages, then
add the approriate layer from meta-openembedded to your bblayers.conf file.

.. note:: Use both for meta-beagleboard, or remove meta-beagleboard and leave meta-ti.

The main source code is checked out in the bsp dir above, and the build dir will default
to oe-core/build-dir unless you choose a different path above.

See the default.xml file for repo and branch details.

Source code
-----------

Download the manifest source here::

  $ git clone https://github.com/VCTLabs/vct-beagleboard-bsp-platform

Using Development and Testing Branches
--------------------------------------

Replace the repo init command above with one of the following:

For developers - jethro

::

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b oe-jethro

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b oe-master

