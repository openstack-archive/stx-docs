.. _developer-guide:

===============
Developer Guide
===============

This section contains the steps for building a StarlingX ISO from Master
branch.

------------
Requirements
------------

The recommended minimum requirements include:

*********************
Hardware Requirements
*********************

A workstation computer with:

-  Processor: x86_64 is the only supported architecture
-  Memory: At least 32GB RAM
-  Hard Disk: 500GB HDD
-  Network: Network adapter with active Internet connection

*********************
Software Requirements
*********************

A workstation computer with:

-  Operating System: Ubuntu 16.04 LTS 64-bit
-  Docker
-  Android Repo Tool
-  Proxy Settings Configured (If Required)

   -  See
      http://lists.starlingx.io/pipermail/starlingx-discuss/2018-July/000136.html
      for more details

-  Public SSH Key

-----------------------------
Development Environment Setup
-----------------------------

This section describes how to set up a StarlingX development system on a
workstation computer. After completing these steps, you will be able to
build a StarlingX ISO image on the following Linux distribution:

-  Ubuntu 16.04 LTS 64-bit

****************************
Update Your Operating System
****************************

Before proceeding with the build, ensure your OS is up to date. You’ll
first need to update the local database list of available packages:

.. code:: sh

   $ sudo apt-get update

******************************************
Installation Requirements and Dependencies
******************************************

^^^
Git
^^^

1. Install the required packages in an Ubuntu host system with:

   .. code:: sh

      $ sudo apt-get install make git curl

2. Make sure to setup your identity

   .. code:: sh

      $ git config --global user.name "Name LastName"
      $ git config --global user.email "Email Address"

^^^^^^^^^
Docker CE
^^^^^^^^^

3. Install the required Docker CE packages in an Ubuntu host system. See
   `Get Docker CE for
   Ubuntu <https://docs.docker.com/install/linux/docker-ce/ubuntu/#os-requirements>`__
   for more information.

^^^^^^^^^^^^^^^^^
Android Repo Tool
^^^^^^^^^^^^^^^^^

4. Install the required Android Repo Tool in an Ubuntu host system. Follow
   the 2 steps in "Installing Repo" section from `Installing
   Repo <https://source.android.com/setup/build/downloading#installing-repo>`__
   to have Andriod Repo Tool installed.

**********************
Install Public SSH Key
**********************

#. Follow these instructions on GitHub to `Generate a Public SSH
   Key <https://help.github.com/articles/connecting-to-github-with-ssh>`__
   and then upload your public key to your GitHub and Gerrit account
   profiles:

   -  `Upload to
      Github <https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account>`__
   -  `Upload to
      Gerrit <https://review.openstack.org/#/settings/ssh-keys>`__

*************************
Install stx-tools Project
*************************

#. Under your $HOME directory, clone the <stx-tools> project

   .. code:: sh

      $ cd $HOME
      $ git clone https://git.starlingx.io/stx-tools

****************************
Create a Workspace Directory
****************************

#. Create a *starlingx* workspace directory on your workstation
   computer. Usually, you’ll want to create it somewhere under your
   user’s home directory.

   .. code:: sh

      $ mkdir -p $HOME/starlingx/

----------------------------------
Build the CentOS Mirror Repository
----------------------------------

This section describes how to build the CentOS Mirror Repository.

*********************************
Setup Repository Docker Container
*********************************

Run the following commands under a terminal identified as "One".

#. Navigate to the *<$HOME/stx-tools>/centos-mirror-tool* project
   directory:

   .. code:: sh

      $ cd $HOME/stx-tools/centos-mirror-tools/

#. If necessary you might have to set http/https proxy in your
   Dockerfile before building the docker image.

   .. code:: sh

      ENV http_proxy " http://your.actual_http_proxy.com:your_port "
      ENV https_proxy " https://your.actual_https_proxy.com:your_port "
      ENV ftp_proxy " http://your.actual_ftp_proxy.com:your_port "
      RUN echo " proxy=http://your-proxy.com:port " >> /etc/yum.conf

#. Build your *<user>:<tag>* base container image with **e.g.**
   *user:centos-mirror-repository*

   .. code:: sh

      $ docker build --tag $USER:centos-mirror-repository --file Dockerfile .

#. Launch a *<user>* docker container using previously created Docker
   base container image *<user>:<tag>* **e.g.**
   *-centos-mirror-repository*. As /localdisk is defined as the workdir
   of the container, the same folder name should be used to define the
   volume. The container will start to run and populate a logs and
   output folders in this directory. The container shall be run from the
   same directory where the other scripts are stored.

   .. code:: sh

      $ docker run -itd --name $USER-centos-mirror-repository --volume $(pwd):/localdisk $USER:centos-mirror-repository

   **Note**: the above command will create the container in background,
   this mean that you need to attach it manually. The advantage of this
   is that you can enter/exit from the container many times as you want.

*****************
Download Packages
*****************

#. Attach to the docker repository previously created

   ::

      $ docker exec -it <CONTAINER ID> /bin/bash

#. Inside Repository Docker container, enter the following command to
   download the required packages to populate the CentOS Mirror
   Repository:

   ::

      # bash download_mirror.sh

#. Monitor the download of packages until it is complete. When download
   is complete, the following message is displayed:

   ::

      totally 17 files are downloaded!
      step #3: done successfully
      IMPORTANT: The following 3 files are just bootstrap versions. Based on them, the workable images
      for StarlingX could be generated by running "update-pxe-network-installer" command after "build-iso"
          - out/stx-r1/CentOS/pike/Binary/LiveOS/squashfs.img
          - out/stx-r1/CentOS/pike/Binary/images/pxeboot/initrd.img
          - out/stx-r1/CentOS/pike/Binary/images/pxeboot/vmlinuz

***************
Verify Packages
***************

#. Verify there are no missing or failed packages:

   ::

      # cat logs/*_missing_*.log
      # cat logs/*_failmove_*.log

#. In case there are missing or failed ones due to network instability
   (or timeout), you should download them manually, to assure you get
   all RPMs listed in
   **rpms_3rdparties.lst**/**rpms_centos.lst**/**rpms_centos3rdparties.lst**.

******************
Packages Structure
******************

The following is a general overview of the packages structure that you
will have after having downloaded the packages

::

   /home/<user>/stx-tools/centos-mirror-tools/output
   └── stx-r1
       └── CentOS
           └── pike
               ├── Binary
               │   ├── EFI
               │   ├── images
               │   ├── isolinux
               │   ├── LiveOS
               │   ├── noarch
               │   └── x86_64
               ├── downloads
               │   ├── integrity
               │   └── puppet
               └── Source

*******************************
Create CentOS Mirror Repository
*******************************

Outside your Repository Docker container, in another terminal identified
as "**Two**", run the following commands:

#. From terminal identified as "**Two**", create a *mirror/CentOS*
   directory under your *starlingx* workspace directory:

   .. code:: sh

      $ mkdir -p $HOME/starlingx/mirror/CentOS/

#. Copy the built CentOS Mirror Repository built under
   *$HOME/stx-tools/centos-mirror-tool* to the *$HOME/starlingx/mirror/*
   workspace directory.

   .. code:: sh

      $ cp -r $HOME/stx-tools/centos-mirror-tools/output/stx-r1/ $HOME/starlingx/mirror/CentOS/


-------------------------
Create StarlingX Packages
-------------------------

*******************************
Setup Building Docker Container
*******************************

#. From terminal identified as "**Two**", create the workspace folder

   .. code:: sh

      $ mkdir -p $HOME/starlingx/workspace

#. Navigate to the '' $HOME/stx-tools'' project directory:

   .. code:: sh

      $ cd $HOME/stx-tools

#. Copy your git options to "toCopy" folder

   .. code:: sh

      $ cp ~/.gitconfig toCOPY

#. Create a *<localrc>* file

   .. code:: sh

      $ cat <<- EOF > localrc
      # tbuilder localrc
      MYUNAME=$USER
      PROJECT=starlingx
      HOST_PREFIX=$HOME/starlingx/workspace
      HOST_MIRROR_DIR=$HOME/starlingx/mirror
      EOF

#. If necessary you might have to set http/https proxy in your
   Dockerfile.centos73 before building the docker image.

   .. code:: sh

      ENV http_proxy  "http://your.actual_http_proxy.com:your_port"
      ENV https_proxy "https://your.actual_https_proxy.com:your_port"
      ENV ftp_proxy "http://your.actual_ftp_proxy.com:your_port"
      RUN echo "proxy=$http_proxy" >> /etc/yum.conf && \
      echo -e "export http_proxy=$http_proxy\nexport https_proxy=$https_proxy\n\
      export ftp_proxy=$ftp_proxy" >> /root/.bashrc

#. Base container setup If you are running in fedora system, you will
   see " .makeenv:88: \**\* missing separator. Stop. " error, to
   continue :

   -  delete the functions define in the .makeenv ( module () { ... } )
   -  delete the line 19 in the Makefile and ( NULL := $(shell bash -c
      "source buildrc ... ).

   .. code:: sh

      $ make base-build

#. Build container setup

   .. code:: sh

      $ make build

#. Verify environment variables

   .. code:: sh

      $ bash tb.sh env

#. Build container run

   .. code:: sh

      $ bash tb.sh run

#. Execute the built container:

   .. code:: sh

      $ bash tb.sh exec

*********************************
Download Source Code Repositories
*********************************

#. From terminal identified as "Two", now inside the Building Docker
   container, Internal environment

   .. code:: sh

      $ eval $(ssh-agent)
      $ ssh-add

#. Repo init

   .. code:: sh

      $ cd $MY_REPO_ROOT_DIR
      $ repo init -u https://git.starlingx.io/stx-manifest -m default.xml

#. Repo sync

   .. code:: sh

      $ repo sync -j`nproc`

#. Tarballs Repository

   .. code:: sh

      $ ln -s /import/mirrors/CentOS/stx-r1/CentOS/pike/downloads/ $MY_REPO/stx/

   Alternatively you can run the populate_downloads.sh script to copy
   the tarballs instead of using a symlink.

   .. code:: sh

      $ populate_downloads.sh /import/mirrors/CentOS/stx-r1/CentOS/pike/

   Outside the container

#. From another terminal identified as "Three", Mirror Binaries

   .. code:: sh

      $ mkdir -p $HOME/starlingx/mirror/CentOS/tis-installer
      $ cp $HOME/starlingx/mirror/CentOS/stx-r1/CentOS/pike/Binary/images/pxeboot/initrd.img $HOME/starlingx/mirror/CentOS/tis-installer/initrd.img-stx-0.2
      $ cp $HOME/starlingx/mirror/CentOS/stx-r1/CentOS/pike/Binary/images/pxeboot/vmlinuz $HOME/starlingx/mirror/CentOS/tis-installer/vmlinuz-stx-0.2
      $ cp $HOME/starlingx/mirror/CentOS/stx-r1/CentOS/pike/Binary/LiveOS/squashfs.img $HOME/starlingx/mirror/CentOS/tis-installer/squashfs.img-stx-0.2

**************
Build Packages
**************

#. Back to the Building Docker container, terminal identified as
   "**Two**"
#. **Temporal!** Build-Pkgs Errors Be prepared to have some missing /
   corrupted rpm and tarball packages generated during
   `Build the CentOS Mirror Repository`_ which will make the next step
   to fail, if that happens please download manually those missing /
   corrupted packages.
#. **Update the symbolic links**

   .. code:: sh

      $ generate-cgcs-centos-repo.sh /import/mirrors/CentOS/stx-r1/CentOS/pike/

#. Build-Pkgs

   .. code:: sh

      $ build-pkgs

#. **Optional!** Generate-Cgcs-Tis-Repo
   This step is optional but will improve performance on subsequent
   builds. The cgcs-tis-repo has the dependency information that
   sequences the build order; To generate or update the information the
   following command needs to be executed after building modified or new
   packages.

   .. code:: sh

      $ generate-cgcs-tis-repo

-------------------
Build StarlingX ISO
-------------------

#. Build-Iso

   .. code:: sh

      $ build-iso

---------------
Build Installer
---------------

To get your StarlingX ISO ready to use, you will need to create the init
files that will be used to boot the ISO as well to boot additional
controllers and compute nodes. Note that this procedure only is needed
in your first build and every time the kernel is upgraded.

Once you had run build-iso, run:

.. code:: sh

   $ build-pkgs --installer

This will build *rpm* and *anaconda* packages. Then run:

.. code:: sh

   $ update-pxe-network-installer

The *update-pxe-network-installer* covers the steps detailed in
*$MY_REPO/stx/stx-metal/installer/initrd/README*. This script will
create three files on
*/localdisk/loadbuild///pxe-network-installer/output*.

::

   new-initrd.img
   new-squashfs.img
   new-vmlinuz

Then, rename them to:

::

   initrd.img-stx-0.2
   squashfs.img-stx-0.2
   vmlinuz-stx-0.2

There are two ways to use these files:

#. Store the files in the */import/mirror/CentOS/tis-installer/* folder
   for future use.
#. Store it in an arbitrary location and modify the
   *$MY_REPO/stx/stx-metal/installer/pxe-network-installer/centos/build_srpm.data*
   file to point to these files.

Now, the *pxe-network-installer* package needs to be recreated and the
ISO regenerated.

.. code:: sh

   $ build-pkgs --clean pxe-network-installer
   $ build-pkgs pxe-network-installer
   $ build-iso

Now your ISO should be able to boot.

****************
Additional Notes
****************

-  In order to get the first boot working this complete procedure needs
   to be done. However, once the init files are created, these can be
   stored in a shared location where different developers can make use
   of them. Updating these files is not a frequent task and should be
   done whenever the kernel is upgraded.
-  StarlingX is in active development, so it is possible that in the
   future the **0.2** version will change to a more generic solution.

---------------
Build Avoidance
---------------

*******
Purpose
*******

Greatly reduce build times after a repo sync for designers working
within a regional office. Starting from a new workspace, build-pkgs
typically requires 3+ hours. Build avoidance typically reduces this step
to ~20min

***********
Limitations
***********

-  Little or no benefit for designers who refresh a pre-existing
   workspace at least daily. (download_mirror.sh, repo sync,
   generate-cgcs-centos-repo.sh, build-pkgs, build-iso). In these cases
   an incremental build (reuse of same workspace without a 'build-pkgs
   --clean') is often just as efficient.
-  Not likely to be useful to solo designers, or teleworkers that wish
   to compile on there home computers. Build avoidance downloads build
   artifacts from a reference build, and WAN speeds are generally to
   slow.

*****************
Method (in brief)
*****************

#. Reference Builds

   -  A server in the regional office performs a regular (daily?),
      automated builds using existing methods. Call these the reference
      builds.
   -  The builds are timestamped, and preserved for some time. (a few
      weeks)
   -  A build CONTEXT is captured. This is a file produced by build-pkgs
      at location '$MY_WORKSPACE/CONTEXT'. It is a bash script that can
      cd to each and every git and checkout the SHA that contributed to
      the build.
   -  For each package built, a file shall capture he md5sums of all the
      source code inputs to the build of that package. These files are
      also produced by build-pkgs at location
      '$MY_WORKSPACE//rpmbuild/SOURCES//srpm_reference.md5'.
   -  All these build products are accessible locally (e.g. a regional
      office) via rsync (other protocols can be added later)

#. Designers

   - Request a build avoidance build. Recommended after you have just
     done a repo sync. e.g.

     ::

        repo sync
        generate-cgcs-centos-repo.sh
        populate_downloads.sh
        build-pkgs --build-avoidance

   - Additional arguments, and/or environment variables, and/or a
     config file unique to the regional office, are used to specify a URL
     to the reference builds.

      - Using a config file to specify location of your reference build

        ::

           mkdir -p $MY_REPO/local-build-data

           cat <<- EOF > $MY_REPO/local-build-data/build_avoidance_source
           # Optional, these are already the default values.
           BUILD_AVOIDANCE_DATE_FORMAT="%Y%m%d"
           BUILD_AVOIDANCE_TIME_FORMAT="%H%M%S"
           BUILD_AVOIDANCE_DATE_TIME_DELIM="T"
           BUILD_AVOIDANCE_DATE_TIME_POSTFIX="Z"
           BUILD_AVOIDANCE_DATE_UTC=1
           BUILD_AVOIDANCE_FILE_TRANSFER="rsync"

           # Required, unique values for each regional office
           BUILD_AVOIDANCE_USR="jenkins"
           BUILD_AVOIDANCE_HOST="stx-builder.mycompany.com"
           BUILD_AVOIDANCE_DIR="/localdisk/loadbuild/jenkins/StarlingX_Reference_Build"
           EOF

      - Using command line args to specify location of your reference
        build

        ::

           build-pkgs --build-avoidance --build-avoidance-dir /localdisk/loadbuild/jenkins/StarlingX_Reference_Build --build-avoidance-host stx-builder.mycompany.com --build-avoidance-user jenkins

   -  Prior to your build attempt, you need to accept the host key. This will prevent rsync failures on a yes/no prompt. (you should only have to do this once)

      ::

         grep -q $BUILD_AVOIDANCE_HOST $HOME/.ssh/known_hosts
         if [ $? != 0 ]; then
         ssh-keyscan $BUILD_AVOIDANCE_HOST >> $HOME/.ssh/known_hosts
         fi


   -  build-pkgs will:

      -  From newest to oldest, scan the CONTEXTs of the various
         reference builds. Select the first (most recent) context which
         satisfies the following requirement. For every git, the SHA
         specified in the CONTEXT is present.
      -  The selected context might be slightly out of date, but not by
         more than a day (assuming daily reference builds).
      -  If the context has not been previously downloaded, then
         download it now. Meaning download select portions of the
         reference build workspace into the designer's workspace. This
         includes all the SRPMS, RPMS, MD5SUMS, and misc supporting
         files. (~10 min over office LAN)
      -  The designer may have additional commits not present in the
         reference build, or uncommitted changes. Affected packages will
         identified by the differing md5sum's, and the package is
         re-built. (5+ min, depending on what packages have changed)

   -  What if no valid reference build is found? Then build-pkgs will fall
      back to a regular build.

****************
Reference Builds
****************

-  The regional office implements an automated build that pulls the
   latest StarlingX software and builds it on a regular basis. e.g. a
   daily. Perhaps implemented by Jenkins, cron, or similar tools.
-  Each build is saved to a unique directory, and preserved for a time
   that is reflective of how long a designer might be expected to work
   on a private branch without syncronizing with the master branch. e.g.
   2 weeks.

- The MY_WORKSPACE directory for the build shall have a common root
  directory, and a leaf directory that is a sortable time stamp. Suggested
  format YYYYMMDDThhmmss. e.g.

  .. code:: sh

     $ sudo apt-get update
     BUILD_AVOIDANCE_DIR="/localdisk/loadbuild/jenkins/StarlingX_Reference_Build"
     BUILD_TIMESTAMP=$(date -u '+%Y%m%dT%H%M%SZ')
     MY_WORKSPACE=${BUILD_AVOIDANCE_DIR}/${BUILD_TIMESTAMP}

-  Designers can access all build products over the internal network of
   the regional office. The current prototype employs rsync. Other
   protocols that can efficiently share/copy/transfer large directories
   of content can be added as needed.

^^^^^^^^^^^^^^
Advanced Usage
^^^^^^^^^^^^^^

Can the reference build itself use build avoidance? Yes
Can it reference itself? Yes.
In either case we advise caution. To protect against any possible
'divergence from reality', you should limit how many steps removed a
build avoidance build is from a full build.

Suppose we want to implement a self referencing daily build, except
that a full build occurs every Saturday. To protect ourselves from a
build failure on Saturday we also want a limit of 7 days since last
full build. You build script might look like this ...

::

   ...
   BUILD_AVOIDANCE_DIR="/localdisk/loadbuild/jenkins/StarlingX_Reference_Build"
   BUILD_AVOIDANCE_HOST="stx-builder.mycompany.com"
   FULL_BUILD_DAY="Saturday"
   MAX_AGE_DAYS=7

   LAST_FULL_BUILD_LINK="$BUILD_AVOIDANCE_DIR/latest_full_build"
   LAST_FULL_BUILD_DAY=""
   NOW_DAY=$(date -u "+%A")
   BUILD_TIMESTAMP=$(date -u '+%Y%m%dT%H%M%SZ')
   MY_WORKSPACE=${BUILD_AVOIDANCE_DIR}/${BUILD_TIMESTAMP}

   # update software
   repo init -u ${BUILD_REPO_URL} -b ${BUILD_BRANCH}
   repo sync --force-sync
   $MY_REPO_ROOT_DIR/stx-tools/toCOPY/generate-cgcs-centos-repo.sh
   $MY_REPO_ROOT_DIR/stx-tools/toCOPY/populate_downloads.sh

   # User can optionally define BUILD_METHOD equal to one of 'FULL', 'AVOIDANCE', or 'AUTO'
   # Sanitize BUILD_METHOD
   if [ "$BUILD_METHOD" != "FULL" ] && [ "$BUILD_METHOD" != "AVOIDANCE" ]; then
       BUILD_METHOD="AUTO"
   fi

   # First build test
   if [ "$BUILD_METHOD" != "FULL" ] && [ ! -L $LAST_FULL_BUILD_LINK ]; then
       echo "latest_full_build symlink missing, forcing full build"
       BUILD_METHOD="FULL"
   fi

   # Build day test
   if [ "$BUILD_METHOD" == "AUTO" ] && [ "$NOW_DAY" == "$FULL_BUILD_DAY" ]; then
       echo "Today is $FULL_BUILD_DAY, forcing full build"
       BUILD_METHOD="FULL"
   fi

   # Build age test
   if [ "$BUILD_METHOD" != "FULL" ]; then
       LAST_FULL_BUILD_DATE=$(basename $(readlink $LAST_FULL_BUILD_LINK) | cut -d '_' -f 1)
       LAST_FULL_BUILD_DAY=$(date -d $LAST_FULL_BUILD_DATE "+%A")
       AGE_SECS=$(( $(date "+%s") - $(date -d $LAST_FULL_BUILD_DATE "+%s") ))
       AGE_DAYS=$(( $AGE_SECS/60/60/24 ))
       if [ $AGE_DAYS -ge $MAX_AGE_DAYS ]; then
           echo "Haven't had a full build in $AGE_DAYS days, forcing full build"
           BUILD_METHOD="FULL"
       fi
       BUILD_METHOD="AVOIDANCE"
   fi

   #Build it
   if [ "$BUILD_METHOD" == "FULL" ]; then
       build-pkgs --no-build-avoidance
   else
       build-pkgs --build-avoidance --build-avoidance-dir $BUILD_AVOIDANCE_DIR --build-avoidance-host $BUILD_AVOIDANCE_HOST --build-avoidance-user $USER
   fi
   if [ $? -ne 0 ]; then
       echo "Build failed in build-pkgs"
       exit 1
   fi

   build-iso
   if [ $? -ne 0 ]; then
       echo "Build failed in build-iso"
       exit 1
   fi

   if [ "$BUILD_METHOD" == "FULL" ]; then
       # A successful full build.  Set last full build symlink.
       if [ -L $LAST_FULL_BUILD_LINK ]; then
           rm -rf $LAST_FULL_BUILD_LINK
       fi
       ln -sf $MY_WORKSPACE $LAST_FULL_BUILD_LINK
   fi
   ...

One final wrinkle.

We can ask build avoidance to preferentially use the full build day
rather than the most recent build, as the reference point of the next
avoidance build via use of '--build-avoidance-day '. e.g. substitute
this line into the above.

::

   build-pkgs --build-avoidance --build-avoidance-dir $BUILD_AVOIDANCE_DIR --build-avoidance-host $BUILD_AVOIDANCE_HOST --build-avoidance-user $USER --build-avoidance-day $FULL_BUILD_DAY

   # or perhaps, with a bit more shuffling of the above script.

   build-pkgs --build-avoidance --build-avoidance-dir $BUILD_AVOIDANCE_DIR --build-avoidance-host $BUILD_AVOIDANCE_HOST --build-avoidance-user $USER --build-avoidance-day $LAST_FULL_BUILD_DAY

The advantage is that our build is never more than one step removed
from a full build (assuming the full build was successful).

The disadvantage is that by end of week the reference build is getting
rather old. During active weeks, builds times might be approaching
that of a full build.
