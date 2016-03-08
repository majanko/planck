# The Planck Linux Distribution
Repositories  for the  the Cross  Build  of Open  Source Software  for
[Qualcomm  Mobile  Data  Modem  (MDM) Application  Processors](https://www.qualcomm.com/products/snapdragon/modems) used in various
[connectivity modules](http://www.peiker.de/en/telematics-connectivity/connectivity-modules.html)
manufactured at Peiker acustic.

## Object
Qualcomm provides  within its  MDM9x series modem  system on  chip  an
ARMv7 core as application processor which comes with an embedded Linux
operating system  that is  derived from  a meanwhile  outdated version
of  the [OpenEmbedded](http://openembedded.org)  build framework.  The
Qualcomm extensions to  this are mostly closed  source, closely linked
to it and must exclusively distributed to official Qualcomm licensees.
This makes it difficult to involve external development partners since
the  source  code  cannot  be shared  with  them  without  significant
modifications.

Third  party software  packages which  are not  directly depending  on
Qualcomm API's can be compiled and  tested on the original open source
Linux  crossbuild   framework  [OpenEmbedded](http://openembedded.org)
instead where Qualcomm open source  software is based on. Non Qualcomm
licensees can do their development  on open third party hardware which
is today available out of the shelf  for very low budget as in example
with the [BeagleBone black](http://beagleboard.org/black).

The  basic  problem  with  this  approach  is  the  lack  respectively
the  version mismatch  of  build environments  which  comes with  open
hardware and the  very specific version of the  build environment used
in  Qualcomm's  MDM9x  SoC's.  So  the  essential  objective  of  this
distribution  is to  liberate open  source software  packages used  in
these processor  cores from their proprietary  counterparts. This will
allow  to exchange  meta  build recipes  for  bitbake without  further
modifications at least .

## Limitations
The given  approach does not  provide access via proprietary  API's to
data  modem  functionality.  Furthermore  with  respect  to  different
hardware, different Linux kernel  versions are incorporated. So source
code e.g. for kernel drivers might not be fully compabible between the
open development  platform e.g. the  BeagleBone black and  an original
Qualcomm MDM9x SoC.

## Preconditions
### Host Operating System
Compilation requires a  possibly fast Linux host PC with  at least 4GB
RAM, 40GB free hard disk space,  preferably an ssd drive and the Linux
distribution as specified by Qualcomm for given SoC. These are

* Ubuntu 10.04 for the MDM9615.LE.5.5 Linux enabled Release for MDM9615 Devices
* Ubuntu 12.04 for the MDM9640.LE.2.0 Linux enabled Release for MDM9640 Devices

Alternatively the build runs with minor changes on

* [Debian Wheezy 7.8](https://www.debian.org/releases/wheezy)

which can be  used for cross compiling Linux images  for both devices.
For the Planck  distribution Debian Wheezy is  considered as reference
host  platform for  this  reason. Be  aware  of servere  compatibility
issues when using a more recent distribution e.g. Debian Jessie.

The majority of build utilities are  compiled from scratch by the meta
build  framework provided  by OpenEmbedded/bitbake.  Neverthess a  few
essential packages are  required initially which have  to be installed
by the following shell command:

    sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
       build-essential chrpath libsdl1.2-dev xterm

Make sure that you use 'bash' as /bin/sh instead of the preinstalled 'dash'
This can be done by the command

    dpkg-reconfigure dash

### Android Repo Script Framework
This distribution is based on
[Android Repo](https://source.android.com/source/using-repo.html)
which must be installed as well. The installation steps in the following
are taken from the
[android website](https://source.android.com/source/downloading.html):

1. Make sure you have a bin/ directory in your home directory and
that it is included in your path:

        mkdir ~/bin
        PATH=~/bin:$PATH

2. Download the Repo tool and ensure that it is executable

        curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
        chmod a+x ~/bin/repo

## Fetching of the initial Source Code
Create a project root directory in your home folder and set the current
working directory to it:

    mkdir planck
    cd planck

Initialize the repo framework

    repo init -u git://github.com/majanko/planck.git -m mdm9640.xml

This  will  setup the  cross  build  environment to  match  Qualcomm's
MDM9640 SoC  which is exclusively supported  initially (status January
2016). Later on other OpenEmbedded releases might be added.

Download the Planck source tree

    repo sync

## Configure the Machine Configuration
The  Planck   distribution  comes   with  the   machine  configuration
preconfigured for the [BeagleBone black](http://beagleboard.org/black)
which  has been  currently excluviley  tested (January  2016). If  you
intend  to cross  compile Linux  images for  other hardware  platforms
change the settings in the file build/conf/local.conf.

## Start the Build
From the project's root folder  invoke the following commands to start
the build for the LTENAD version 2 development image:

    source oe-core/oe-init-build-env
    bitbake ltenad2

In case you need to build development image with Ethernet via USB, port
forwarding and static IP addresses please type:

    source oe-core/oe-init-build-env
    bitbake ltenad2-mcu-cdc-ncm

In both cases it will  start the so  called meta build.  The vast majority  of the
source code is only now downloaded  from various locations on the web.
All downloaded software will be stored under directory build/downlads.
In case  you work  in a  larger team  you might  consider to  inject a
previously fetched version  of this directory e.g. by  mounting it via
the network filesystem.

The build  takes a considerable  amount of  time. We recommend  to use
modern hardware for this reason. We have preconfigured the setup under
build/conf/local.conf  to use  eight  tasks for  both,  the number  of
bitbake threads to be executed in parallel and the number of processes
used in make.  If you are compiling  on a virtual machine  or on older
hardware you might consider to use more appropriate values.

## License
This software can be used under the terms of the MIT License.

2016-01-12 Otto Linnemann

2016-03-08 Marian Vancik
