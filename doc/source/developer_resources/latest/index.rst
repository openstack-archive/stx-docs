===========================
Developer guide stx.2019.05
===========================

This section contains the steps for building a StarlingX ISO from
the "latest" StarlingX software (i.e. the "under development" branch).
If this is not the software you want to use, see the
:doc:`available developer guides </developer_resources/index>` for
StarlingX.

------------
Requirements
------------

The recommended minimum requirements include:

*********************
Hardware requirements
*********************

A workstation computer with:

-  Processor: x86_64 is the only supported architecture
-  Memory: At least 32GB RAM
-  Hard Disk: 500GB HDD
-  Network: Network adapter with active Internet connection

*********************
Software requirements
*********************

A workstation computer with:

-  Operating System: Ubuntu 16.04 LTS 64-bit
-  Docker
-  Android Repo Tool
-  Proxy settings configured (if required)

   -  See
      http://lists.starlingx.io/pipermail/starlingx-discuss/2018-July/000136.html
      for more details

-  Public SSH key

-----------------------------
Development environment setup
-----------------------------

This section describes how to set up a StarlingX development system on a
workstation computer. After completing these steps, you can
build a StarlingX ISO image on the following Linux distribution:

-  Ubuntu 16.04 LTS 64-bit

****************************
Update your operating system
****************************

Before proceeding with the build, ensure your Ubuntu distribution is up to date.
You first need to update the local database list of available packages:

.. code:: sh

   $ sudo apt-get update

******************************************
Installation requirements and dependencies
******************************************

^^^^
User
^^^^

1. Make sure you are a non-root user with sudo enabled when you build the
   StarlingX ISO. You also need to either use your existing user or create a
   separate *<user>*:

   .. code:: sh

      $ sudo useradd -m -d /home/<user> <user>

2. Your *<user>* should have sudo privileges:

   .. code:: sh

      $ sudo sh -c "echo '<user> ALL=(ALL:ALL) ALL' >> /etc/sudoers"
      $ sudo su -c <user>

^^^
Git
^^^

3. Install the required packages on the Ubuntu host system:

   .. code:: sh

      $ sudo apt-get install make git curl

4. Make sure to set up your identity using the following two commands.
   Be sure to provide your actual name and email address:

   .. code:: sh

      $ git config --global user.name "Name LastName"
      $ git config --global user.email "Email Address"

^^^^^^^^^
Docker CE
^^^^^^^^^

5. Install the required Docker CE packages in the Ubuntu host system. See
   `Get Docker CE for
   Ubuntu <https://docs.docker.com/install/linux/docker-ce/ubuntu/#os-requirements>`__
   for more information.

6. Log out and log in to add your *<user>* to the Docker group:

    .. code:: sh

       $ sudo usermod -aG docker <user>

^^^^^^^^^^^^^^^^^
Android Repo Tool
^^^^^^^^^^^^^^^^^

7. Install the required Android Repo Tool in the Ubuntu host system. Follow
   the steps in the `Installing
   Repo <https://source.android.com/setup/build/downloading#installing-repo>`__
   section.

**********************
Install public SSH key
**********************

#. Follow these instructions on GitHub to `Generate a Public SSH
   Key <https://help.github.com/articles/connecting-to-github-with-ssh>`__.
   Then upload your public key to your GitHub and Gerrit account
   profiles:

   -  `Upload to
      Github <https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account>`__
   -  `Upload to
      Gerrit <https://review.openstack.org/#/settings/ssh-keys>`__

****************************
Create a workspace directory
****************************

#. Create a *starlingx* workspace directory on your system.
   Best practices dictate creating the workspace directory
   in your $HOME directory:

   .. code:: sh

      $ mkdir -p $HOME/starlingx/

*************************
Install stx-tools project
*************************

#. Under your $HOME directory, clone the <stx-tools> project:

   .. code:: sh

      $ cd $HOME
      $ git clone https://git.starlingx.io/stx-tools

#. Navigate to the *<$HOME/stx-tools>* project
   directory:

   .. code:: sh

      $ cd $HOME/stx-tools/

-----------------------------
Prepare the base Docker image
-----------------------------

StarlingX base Docker image handles all steps related to StarlingX ISO
creation. This section describes how to customize the base Docker image
building process.

********************
Configuration values
********************

You can customize values for the StarlingX base Docker image using a
text-based configuration file named ``localrc``:

- ``HOST_PREFIX`` points to the directory that hosts the 'designer'
  subdirectory for source code, the 'loadbuild' subdirectory for
  the build environment, generated RPMs, and the ISO image.
- ``HOST_MIRROR_DIR`` points to the directory that hosts the CentOS mirror
  repository.

^^^^^^^^^^^^^^^^^^^^^^^^^^
localrc configuration file
^^^^^^^^^^^^^^^^^^^^^^^^^^

Create your ``localrc`` configuration file. For example:

    .. code:: sh

       # tbuilder localrc
       MYUNAME=<your user name>
       PROJECT=starlingx
       HOST_PREFIX=$HOME/starlingx/workspace
       HOST_MIRROR_DIR=$HOME/starlingx/mirror

***************************
Build the base Docker image
***************************

Once the ``localrc`` configuration file has been customized, it is time
to build the base Docker image.

#. If necessary, you might have to set http/https proxy in your
   Dockerfile before building the docker image:

   .. code:: sh

      ENV http_proxy " http://your.actual_http_proxy.com:your_port "
      ENV https_proxy " https://your.actual_https_proxy.com:your_port "
      ENV ftp_proxy " http://your.actual_ftp_proxy.com:your_port "
      RUN echo " proxy=http://your-proxy.com:port " >> /etc/yum.conf

#. The ``tb.sh`` script automates the Base Docker image build:

   .. code:: sh

       ./tb.sh create

----------------------------------
Build the CentOS mirror repository
----------------------------------

The creation of the StarlingX ISO relies on a repository of RPM Binaries,
RPM Sources, and Tar Compressed files. This section describes how to build
this CentOS mirror repository.

*******************************
Run repository Docker container
*******************************

| Run the following commands under a terminal identified as "**One**":

#. Navigate to the *$HOME/stx-tools/centos-mirror-tool* project
   directory:

   .. code:: sh

      $ cd $HOME/stx-tools/centos-mirror-tools/

#. Launch the Docker container using the previously created base Docker image
   *<repository>:<tag>*. As /localdisk is defined as the workdir of the
   container, you should use the same folder name to define the volume.
   The container starts to run and populate 'logs' and 'output' folders in
   this directory. The container runs from the same directory in which the
   scripts are stored.

   .. code:: sh

      $ docker run -it --volume $(pwd):/localdisk local/$USER-stx-builder:7.4 bash

*****************
Download packages
*****************

#. Inside the Docker container, enter the following commands to download
   the required packages to populate the CentOS mirror repository:

   ::

      # cd localdisk && bash download_mirror.sh

#. Monitor the download of packages until it is complete. When the download
   is complete, the following message appears:

   ::

      totally 17 files are downloaded!
      step #3: done successfully
      IMPORTANT: The following 3 files are just bootstrap versions. Based on them, the workable images
      for StarlingX could be generated by running "update-pxe-network-installer" command after "build-iso"
          - out/stx-r1/CentOS/pike/Binary/LiveOS/squashfs.img
          - out/stx-r1/CentOS/pike/Binary/images/pxeboot/initrd.img
          - out/stx-r1/CentOS/pike/Binary/images/pxeboot/vmlinuz

***************
Verify packages
***************

#. Verify no missing or failed packages exist:

   ::

      # cat logs/*_missing_*.log
      # cat logs/*_failmove_*.log

#. In case missing or failed packages do exist, which is usually caused by
   network instability (or timeout), you need to download the packages
   manually.
   Doing so assures you get all RPMs listed in
   *rpms_3rdparties.lst*/*rpms_centos.lst*/*rpms_centos3rdparties.lst*.

******************
Packages structure
******************

The following is a general overview of the packages structure resulting
from downloading the packages:

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
Create CentOS mirror repository
*******************************

Outside your Repository Docker container, in another terminal identified
as "**Two**", run the following commands:

#. From terminal identified as "**Two**", create a *mirror/CentOS*
   directory under your *starlingx* workspace directory:

   .. code:: sh

      $ mkdir -p $HOME/starlingx/mirror/CentOS/

#. Copy the built CentOS Mirror Repository built under
   *$HOME/stx-tools/centos-mirror-tool* to the *$HOME/starlingx/mirror/*
   workspace directory:

   .. code:: sh

      $ cp -r $HOME/stx-tools/centos-mirror-tools/output/stx-r1/ $HOME/starlingx/mirror/CentOS/


-------------------------
Create StarlingX packages
-------------------------

*****************************
Run building Docker container
*****************************

#. From the terminal identified as "**Two**", create the workspace folder:

   .. code:: sh

      $ mkdir -p $HOME/starlingx/workspace

#. Navigate to the *$HOME/stx-tools* project directory:

   .. code:: sh

      $ cd $HOME/stx-tools

#. Verify environment variables:

   .. code:: sh

      $ bash tb.sh env

#. Run the building Docker container:

   .. code:: sh

      $ bash tb.sh run

#. Execute the buiding Docker container:

   .. code:: sh

      $ bash tb.sh exec

*********************************
Download source code repositories
*********************************

#. From the terminal identified as "**Two**", which is now inside the
   Building Docker container, start the internal environment:

   .. code:: sh

      $ eval $(ssh-agent)
      $ ssh-add

#. Use the repo tool to create a local clone of the stx-manifest
   Git repository based on the "master" branch:

   .. code:: sh

      $ cd $MY_REPO_ROOT_DIR
      $ repo init -u https://git.starlingx.io/stx-manifest -m default.xml

#. Synchronize the repository:

   .. code:: sh

      $ repo sync -j`nproc`

#. Create a tarballs repository:

   .. code:: sh

      $ ln -s /import/mirrors/CentOS/stx-r1/CentOS/pike/downloads/ $MY_REPO/stx/

   Alternatively, you can run the "populate_downloads.sh" script to copy
   the tarballs instead of using a symlink:

   .. code:: sh

      $ populate_downloads.sh /import/mirrors/CentOS/stx-r1/CentOS/pike/

   Outside the container

#. From another terminal identified as "**Three**", create mirror binaries:

   .. code:: sh

      $ mkdir -p $HOME/starlingx/mirror/CentOS/stx-installer
      $ cp $HOME/starlingx/mirror/CentOS/stx-r1/CentOS/pike/Binary/images/pxeboot/initrd.img $HOME/starlingx/mirror/CentOS/stx-installer/initrd.img
      $ cp $HOME/starlingx/mirror/CentOS/stx-r1/CentOS/pike/Binary/images/pxeboot/vmlinuz $HOME/starlingx/mirror/CentOS/stx-installer/vmlinuz
      $ cp $HOME/starlingx/mirror/CentOS/stx-r1/CentOS/pike/Binary/LiveOS/squashfs.img $HOME/starlingx/mirror/CentOS/stx-installer/squashfs.img

**************
Build packages
**************

#. Go back to the terminal identified as "**Two**", which is the Building Docker container.

#. **Temporal!** Build-Pkgs Errors. Be prepared to have some missing /
   corrupted rpm and tarball packages generated during
   `Build the CentOS Mirror Repository`_, which will cause the next step
   to fail. If that step does fail, manually download those missing /
   corrupted packages.

#. Update the symbolic links:

   .. code:: sh

      $ generate-cgcs-centos-repo.sh /import/mirrors/CentOS/stx-r1/CentOS/pike/

#. Build the packages:

   .. code:: sh

      $ build-pkgs

#. **Optional!** Generate-Cgcs-Tis-Repo:

   While this step is optional, it improves performance on subsequent
   builds. The cgcs-tis-repo has the dependency information that
   sequences the build order. To generate or update the information, you
   need to execute the following command after building modified or new
   packages.

   .. code:: sh

      $ generate-cgcs-tis-repo

-------------------
Build StarlingX ISO
-------------------

#. Build the image:

   .. code:: sh

      $ build-iso

---------------
Build installer
---------------

To get your StarlingX ISO ready to use, you must create the initialization
files used to boot the ISO, additional controllers, and compute nodes.

**NOTE:** You only need this procedure during your first build and
every time you upgrade the kernel.

After running "build-iso", run:

.. code:: sh

   $ build-pkgs --installer

This builds *rpm* and *anaconda* packages. Then run:

.. code:: sh

   $ update-pxe-network-installer

The *update-pxe-network-installer* covers the steps detailed in
*$MY_REPO/stx/stx-metal/installer/initrd/README*. This script
creates three files on
*/localdisk/loadbuild/pxe-network-installer/output*.

::

   new-initrd.img
   new-squashfs.img
   new-vmlinuz

Rename the files as follows:

::

   initrd.img
   squashfs.img
   vmlinuz

Two ways exist for using these files:

#. Store the files in the */import/mirror/CentOS/stx-installer/* folder
   for future use.
#. Store the files in an arbitrary location and modify the
   *$MY_REPO/stx/stx-metal/installer/pxe-network-installer/centos/build_srpm.data*
   file to point to these files.

Recreate the *pxe-network-installer* package and rebuild the image:

.. code:: sh

   $ build-pkgs --clean pxe-network-installer
   $ build-pkgs pxe-network-installer
   $ build-iso

Your ISO image should be able to boot.

****************
Additional notes
****************

-  In order to get the first boot working, this complete procedure needs
   to be done. However, once the init files are created, these can be
   stored in a shared location where different developers can make use
   of them. Updating these files is not a frequent task and should be
   done whenever the kernel is upgraded.
-  StarlingX is in active development.  Consequently, it is possible that in the
   future the **0.2** version will change to a more generic solution.

---------------
Build avoidance
---------------

*******
Purpose
*******

Greatly reduce build times after using "repo" to syncronized a local
repository with an upstream source (i.e. "repo sync").
Build avoidance works well for designers working
within a regional office. Starting from a new workspace, "build-pkgs"
typically requires three or more hours to complete. Build avoidance
reduces this step to approximately 20 minutes.

***********
Limitations
***********

-  Little or no benefit for designers who refresh a pre-existing
   workspace at least daily (e.g. download_mirror.sh, repo sync,
   generate-cgcs-centos-repo.sh, build-pkgs, build-iso). In these cases,
   an incremental build (i.e. reuse of same workspace without a "build-pkgs
   --clean") is often just as efficient.
-  Not likely to be useful to solo designers, or teleworkers that wish
   to compile on using their home computers. Build avoidance downloads build
   artifacts from a reference build, and WAN speeds are generally too
   slow.

*****************
Method (in brief)
*****************

#. Reference Builds

   -  A server in the regional office performs regular (e.g. daily)
      automated builds using existing methods. These builds are called
      "reference builds".
   -  The builds are timestamped and preserved for some time (i.e. a
      number of weeks).
   -  A build CONTEXT, which is a file produced by "build-pkgs"
      at location *$MY_WORKSPACE/CONTEXT*, is captured. It is a bash script that can
      cd to each and every Git and checkout the SHA that contributed to
      the build.
   -  For each package built, a file captures the md5sums of all the
      source code inputs required to build that package. These files are
      also produced by "build-pkgs" at location
      *$MY_WORKSPACE//rpmbuild/SOURCES//srpm_reference.md5*.
   -  All these build products are accessible locally (e.g. a regional
      office) using "rsync".

      **NOTE:** Other protocols can be added later.

#. Designers

   - Request a build avoidance build. Recommended after you have
     done synchronized the repository (i.e. "repo sync").

     ::

        repo sync
        generate-cgcs-centos-repo.sh
        populate_downloads.sh
        build-pkgs --build-avoidance

   - Use combinations of additional arguments, environment variables, and a
     configuration file unique to the regional office to specify an URL
     to the reference builds.

      - Using a configuration file to specify the location of your reference build:

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

      - Using command-line arguments to specify the location of your reference
        build:

        ::

           build-pkgs --build-avoidance --build-avoidance-dir /localdisk/loadbuild/jenkins/StarlingX_Reference_Build --build-avoidance-host stx-builder.mycompany.com --build-avoidance-user jenkins

   -  Prior to your build attempt, you need to accept the host key.
      Doing so prevents "rsync" failures on a "yes/no" prompt.
      You only have to do this once.

      ::

         grep -q $BUILD_AVOIDANCE_HOST $HOME/.ssh/known_hosts
         if [ $? != 0 ]; then
         ssh-keyscan $BUILD_AVOIDANCE_HOST >> $HOME/.ssh/known_hosts
         fi


   -  "build-pkgs" does the following:

      -  From newest to oldest, scans the CONTEXTs of the various
         reference builds. Selects the first (i.e. most recent) context that
         satisfies the following requirement: every Git the SHA
         specifies in the CONTEXT is present.
      -  The selected context might be slightly out of date, but not by
         more than a day. This assumes daily reference builds are run.
      -  If the context has not been previously downloaded, then
         download it now. This means you need to download select portions of the
         reference build workspace into the designer's workspace. This
         includes all the SRPMS, RPMS, MD5SUMS, and miscellaneous supporting
         files. Downloading these files usually takes about 10 minutes
         over an office LAN.
      -  The designer could have additional commits or uncommitted changes
         not present in the reference builds. Affected packages are
         identified by the differing md5sum's.  In these cases, the packages
         are re-built.  Re-builds usually take five or more minutes,
         depending on the packages that have changed.

   -  What if no valid reference build is found? Then build-pkgs will fall
      back to a regular build.

****************
Reference builds
****************

-  The regional office implements an automated build that pulls the
   latest StarlingX software and builds it on a regular basis (e.g.
   daily builds).  Jenkins, cron, or similar tools can trigger these builds.
-  Each build is saved to a unique directory, and preserved for a time
   that is reflective of how long a designer might be expected to work
   on a private branch without syncronizing with the master branch.
   This takes about two weeks.

- The *MY_WORKSPACE* directory for the build shall have a common root
  directory, and a leaf directory that is a sortable time stamp. The
  suggested format is *YYYYMMDDThhmmss*.

  .. code:: sh

     $ sudo apt-get update
     BUILD_AVOIDANCE_DIR="/localdisk/loadbuild/jenkins/StarlingX_Reference_Build"
     BUILD_TIMESTAMP=$(date -u '+%Y%m%dT%H%M%SZ')
     MY_WORKSPACE=${BUILD_AVOIDANCE_DIR}/${BUILD_TIMESTAMP}

-  Designers can access all build products over the internal network of
   the regional office. The current prototype employs "rsync". Other
   protocols that can efficiently share, copy, or transfer large directories
   of content can be added as needed.

^^^^^^^^^^^^^^
Advanced usage
^^^^^^^^^^^^^^

Can the reference build itself use build avoidance? Yes it can.
Can it reference itself? Yes it can.
In both these cases, caution is advised. To protect against any possible
'divergence from reality', you should limit how many steps you remove
a build avoidance build from a full build.

Suppose we want to implement a self-referencing daily build in an
environment where a full build already occurs every Saturday.
To protect ourselves from a
build failure on Saturday we also want a limit of seven days since
the last full build. Your build script might look like this ...

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

A final note....

To use the full build day as your avoidance build reference point,
modify the "build-pkgs" commands above to use "--build-avoidance-day ",
as shown in the following two examples:

::

   build-pkgs --build-avoidance --build-avoidance-dir $BUILD_AVOIDANCE_DIR --build-avoidance-host $BUILD_AVOIDANCE_HOST --build-avoidance-user $USER --build-avoidance-day $FULL_BUILD_DAY

   # Here is another example with a bit more shuffling of the above script.

   build-pkgs --build-avoidance --build-avoidance-dir $BUILD_AVOIDANCE_DIR --build-avoidance-host $BUILD_AVOIDANCE_HOST --build-avoidance-user $USER --build-avoidance-day $LAST_FULL_BUILD_DAY

The advantage is that our build is never more than one step removed
from a full build. This assumes the full build was successful.

The disadvantage is that by the end of the week, the reference build is getting
rather old. During active weeks, build times could approach build times for
full builds.
