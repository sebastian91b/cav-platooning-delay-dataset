# Simulation Environment Installation Guide

The following appendix contains a non-official installation guide intended to support reproducibility of the experimental environment used in this study.

The installation procedure presented in this appendix was developed based on:
- official documentation of OMNeT++, Veins, Plexe, SUMO and ComFASE,
- publicly available GitHub repositories,
- the author's own installation notes.

Terminal commands comes from the sources mentioned above and other publicly available sources. These software developers published their own READMEs and guides that are much better and describe the entire installation process in greater detail. They are publicly available and I strongly recommend to read them first before using this guide.  


************************************
***Preparing packages to workflow***
************************************
************************************

I use:

catalogue for Omnet++:
/home/platooning/omnetpp-5.6.3

SUMO by sumo apt install:
/usr/bin/sumo
/usr/bin/sumo-gui

catalogue for ComFASE:
/home/platooning/ComFASE
/home/platooning/ComFASE/comfase
/home/platooning/ComFASE/veins
/home/platooning/ComFASE/plexe

Environment loads with: source ~/omnetpp-5.6.3/setenv, then PATH vars:

source ~/omnetpp-5.6.3/setenv
export SUMO_HOME=/usr/share/sumo
export CPLUS_INCLUDE_PATH=$HOME/ComFASE/comfase/src
export LD_LIBRARY_PATH=$HOME/ComFASE/comfase/out/gcc-release/src:$LD_LIBRARY_PATH
export OMNETPP_DEBUGGER=

sudo apt update
sudo apt upgrade -y

sudo apt install -y \
build-essential gcc g++ make perl \
qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools \
bison flex \
libxml2-dev zlib1g-dev \
tcl-dev tk-dev \
default-jre \
curl wget \
mesa-utils

I recommend doing it as below. Steps I did:

1. Make Omnetpp-5.6.3 (possible problem: OSG when cofigure, turn OSG off)
2. Make Veins
3. Install SUMO (possible problem: Could not connect to TraCI server)
4. Make Plexe (possible problem: Python2)
5. Make ComFASE

***************************************************************************************************
**********Install omenetpp-5.6.3 (only this version was tested with ComFASE!)*******************
***************************************************************************************************

wget https://github.com/omnetpp/omnetpp/releases/download/omnetpp-5.6.3/omnetpp-5.6.3-src-linux.tgz
tar -xvf omnetpp-5.6.3-src-linux.tgz

cd omnetpp-5.6.3

source setenv
./configure
make 

*************IF FAIL****************

echo "WITH_OSG=no" >> configure.user

./configure

make

************************************

************************************
*******Install veins-5.1************
************************************

cd ~
git clone -b veins-5.1 https://github.com/sommer/veins.git
cd veins
source ~/omnetpp-5.6.3/setenv
./configure
make

*****************************************************
HERE DO MAKE AGAIN IF NEEDED (it likes to crush here)
*****************************************************
******Make sure you use correct Python version*******
*****************************************************
*******************Install SUMO**********************
*****************************************************

sudo apt update
sudo apt install -y sumo sumo-tools sumo-doc

*****************************************************
****************Install plexe************************
*****************************************************

cd ~
git clone -b plexe-3.0a2 https://github.com/michele-segata/plexe.git
source ~/omnetpp-5.6.3/setenv
./configure --with-veins ~/veins
make

******************************************************
***************Install ComFASE************************
******************************************************

cd ~
git clone https://github.com/RISE-Dependable-Transport-Systems/ComFASE.git
cd ~/ComFASE/comfase
source ~/omnetpp-5.6.3/setenv
./configure
make

*********************************************************
***Here it likes to crash because of hardcoded include***
*********************************************************

IN "cc" FILES:
~/ComFASE/veins/src/veins/base/phyLayer/BasePhyLayer.cc
~/ComFASE/veins/src/veins/base/toolbox/SignalUtils.cc
~/ComFASE/veins/src/veins/base/connectionManager/ChannelAccess.cc

CHANGE include "/opt/sim/Dev-3/comfase/src/comfase/injectorVeins/injectorV.h"

TO include "comfase/injectorVeins/injectorV.h"

AND IN "ini" FILES:
~/ComFASE/plexe/examples/platooning/omnetpp.ini
~/ComFASE/plexe/examples/platooning_comfase/omnetpp.ini

CHANGE include /opt/sim/Dev-3/comfase/src/comfase/injectorVeins/injectorV.ini

TO include /home/platooning/ComFASE/comfase/src/comfase/injectorVeins/injectorV.ini

SET PATH VARIABLE IN TERMINAL:
export CPLUS_INCLUDE_PATH=$HOME/ComFASE/comfase/src

***********************************************************
***********TRY AGAIN***************************************
***********************************************************

cd ~/ComFASE/veins
source ~/omnetpp-5.6.3/setenv
export CPLUS_INCLUDE_PATH=$HOME/ComFASE/comfase/src
make clean
./configure
make

***********************************************************
***********Instal plexe-with-veins again*******************
***********************************************************

cd ~/ComFASE/plexe
source ~/omnetpp-5.6.3/setenv
./configure --with-veins ~/ComFASE/veins
make

source ~/omnetpp-5.6.3/setenv
export SUMO_HOME=/usr/share/sumo
export CPLUS_INCLUDE_PATH=$HOME/ComFASE/comfase/src
export OMNETPP_DEBUGGER=
cd ~/ComFASE/plexe/examples/platooning_comfase
./run -u Qtenv

***********************************************************
*****If not working rebuild veins with ComFASE*************
***********************************************************

cd ~/ComFASE/veins
source ~/omnetpp-5.6.3/setenv
export CPLUS_INCLUDE_PATH=$HOME/ComFASE/comfase/src
export LIBRARY_PATH=$HOME/ComFASE/comfase/out/gcc-release/src
nano src/Makefile


**********IN FILE CHANGE**************************************

LIBS = -L$(HOME)/ComFASE/comfase/out/gcc-release/src -lcomfase

**************************************************************

make clean
make

*************************************************************
*****If not working change include path in INI file**********
*************************************************************

nano ~/ComFASE/plexe/examples/platooning_comfase/omnetpp.ini
include /home/platooning/ComFASE/comfase/src/comfase/injectorVeins/injectorV.ini
ned-path = .;../../src;../../../veins/src;../../../comfase/src

*************************************************************
*****Try open platooning example INI once again using********
*************************************************************

source ~/omnetpp-5.6.3/setenv
export SUMO_HOME=/usr/share/sumo
export CPLUS_INCLUDE_PATH=$HOME/ComFASE/comfase/src
export LD_LIBRARY_PATH=$HOME/ComFASE/comfase/out/gcc-release/src:$LD_LIBRARY_PATH
export OMNETPP_DEBUGGER=
cd ~/ComFASE/plexe/examples/platooning_comfase
./run -u Qtenv

It should work at this point

*************************************************************
