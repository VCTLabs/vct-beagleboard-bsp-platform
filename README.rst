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
  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b oe-dunfell
  $ repo sync

At the end of the commands you have every metadata you need to start work with.

Update existing workspace
-------------------------

In order to bring all the repositories up to date with upstream::

  $ cd clonepi-bsp
  $ repo sync

If you have local changes, you might also need::

  $ repo rebase

Repo tips
---------

Some info on how to customize your sync:

  | ``-j JOBS, --jobs=JOBS``  projects to fetch simultaneously (default 4)
  | ``--no-clone-bundle``     disable use of /clone.bundle on HTTP/HTTPS
  | ``--no-tags``             don't fetch tags

Fastest full sync::

  $ repo sync --no-tags --no-clone-bundle

Smallest/fastest sync::

  $ repo init -u https://github.com/VCTLabs/vct-clonepi-bsp-platform -b oe-dunfell --no-clone-bundle --depth=1
  $ repo sync --no-tags --no-clone-bundle --current-branch


Image build
-----------

To start a simple image build::

  $ cd oe-core
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to beaglebone
  $ bitbake core-image-minimal

You can use any directory (build-dir above) to host your build.  The above commands will
build an image for beaglebone using the oe-core and either meta-ti BSP machine config or
meta-beagleboard BSP (plus meta-ti deps).  If you have unbuildable packages, then
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

For developers - dunfell

::

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b oe-mickledore

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b oe-master

