==============================
 VCT Beagleboard BSP Manifest
==============================

The various branches (other than this one) available here will configure the repo build
for the appropriate branches in each repository and clone them in the typical fashion,
ie, with either poky or oe-core as the parent directory and the additional metadata
layers underneath (as documented in the upstream setup).

The 2 main choices are:

* The yocto BSP (and poky reference distribution) with meta-openembedded metadata
  plus (optional) TI and beagleboard.org BSP's and meta-small-arm-extra

* The openembedded-core metadata with choice of TI and beagleboard.org BSP's, plus
  meta-openembedded metadata and meta-small-arm-extra for additional recipes

  - The more recent release layers have additional metadata (layers) such as
    meta-uav and meta-browsers; add to bblayers.conf as needed
  - There are additional kernel and u-boot recipes in meta-small-arm-extra
    based on the latest patches and mainline branches from the `LinuxOnArm wiki`_

Note that oe-core will build "distroless" by default, however, you can set
DISTRO = "vctlabs" in your local.conf if you want the linux-bb-kernel to
be the default.  See the `meta-small-arm-extra README file`_ for manual config
setup for the extra kernel recipes.

.. _LinuxOnArm wiki: https://eewiki.net/display/linuxonarm/BeagleBone+Black
.. _meta-small-arm-extra README file: https://github.com/sarnold/meta-small-arm-extra

There are 4 main branches for each of the above choices: fido, jethro, krogoth, and master.
Select the main build branch using the github branch button above, which will select the
correct manifest branches and BSP/metadata using the respective branches in this
repo as shown below.

Follow the steps in the readme for the branch you select, then use the same branch
name with the "repo init" command.  This will checkout the
corresponding branch HEAD commits on the configured branch for each repository.

You can get status and more using the ``repo`` command in the top level directory
once you've run the ``repo init`` and ``repo sync`` commands.  Try::

  $ repo info
  $ repo show
  $ repo status

See the default.xml file for repo and branch details; the following example is generic
so go back and select a branch from this repo.

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
  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b poky-fido
  $ repo sync

At the end of the above commands you have all the metadata you need to start
building with poky and meta-oe on fido branches.

Update existing workspace
-------------------------

In order to bring all the repositories up to date with upstream::

  $ cd <my_bsp_dir>
  $ repo sync  # needs local tracking branches

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

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b poky-fido --no-clone-bundle --depth=1
  $ repo sync --no-tags --no-clone-bundle --current-branch

Configure and build
-------------------

To start a simple image build::

  $ cd poky
  $ source ./oe-init-build-env build-dir  # you choose name of build-dir
  $ ${EDITOR} conf/local.conf             # set MACHINE to beaglebone
  $ bitbake core-image-minimal

You can use any directory (build-dir above) to host your build. The above commands will build an image for beaglebone using the core yocto BSP machine config and the default yocto-linux kernel. You can replace the default BSP config with either meta-ti or the meta-beagleboard BSP. This will provide a more Beagle-centric set of defaults for kernel and bootloader, as well as a bigger selection of kernels and TI support tools.

The main source code is checked out in the bsp dir above, and the build dir will default
to poky/build-dir unless you choose a different path above.

Source code
-----------

Download the manifest source here::

  $ git clone https://github.com/VCTLabs/vct-beagleboard-bsp-platform

Using Development and Testing/Release Branches
----------------------------------------------

Replace the repo init command above with one of the following:

For developers - krogoth

::

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b poky-krogoth

For intrepid developers and testers - master

Patches are typically merged into master-next and then are merged into master
after a testing and comment period. It’s possible that master has
something you want or need.  But it’s also possible that using master
breaks something that was working before.  Use with caution.

::

  $ repo init -u https://github.com/VCTLabs/vct-beagleboard-bsp-platform -b poky-master


