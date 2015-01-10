# Install Gazebo from source (Ubuntu and Mac)

## Install Gazebo from source on Ubuntu

### Prerequisites

Make sure you have removed the Ubuntu pre-compiled binaries before installing from source:

    sudo apt-get remove 'gazebo-*' 'sdformat-*'

We also recommend ROS users remove older ROS versions of Gazebo:

    sudo apt-get remove ros-hydro-simulator-gazebo ros-indigo-simulator-gazebo

If you have previously installed from source, be sure you are installing to the same path location or that you have removed the previous installation from source version manually.

As a side note, default install locations:

  1. Pre-compiled Ubuntu Binaries : /usr/bin/gazebo

  2. Default source install : /usr/local/bin/gazebo

### ROS Users

When building Gazebo, we recommend you do not have your <tt>/opt/ros/*/setup.sh</tt> file sourced, as it has been seen to add the wrong libraries to the Gazebo build.

### Install Required Dependencies

Install prerequisites.  A clean Ubuntu system will need:

  **Precise**

        sudo apt-get install build-essential libtinyxml-dev libboost-all-dev cmake mercurial pkg-config libprotoc-dev libprotobuf-dev protobuf-compiler libqt4-dev libtar-dev libcurl4-openssl-dev libcegui-mk2-dev libopenal-dev libtbb-dev libswscale-dev libavformat-dev libavcodec-dev libogre-dev libgts-dev libltdl3-dev playerc++ libxml2-dev libfreeimage-dev freeglut3-dev

  **Trusty**

        sudo apt-get install build-essential libtinyxml-dev libboost-all-dev cmake mercurial pkg-config libprotoc-dev libprotobuf-dev protobuf-compiler libqt4-dev libtar-dev libcurl4-openssl-dev libcegui-mk2-dev libopenal-dev libtbb-dev libswscale-dev libavformat-dev libavcodec-dev libogre-1.8-dev libgts-dev libltdl3-dev playerc++ libxml2-dev libfreeimage-dev freeglut3-dev


### Optional Physics Engines

#### Trusty

**Release Note:** in order to use DART, a full compilation of Gazebo from source is needed (as detailed in this document). The .deb packages are only shipping the ODE, Bullet, and Simbody physics engines.

   ***DART Support***
 
   Support for [DART](http://dartsim.github.io/) version 4.1 is integrated into Gazebo 4.0. In an Ubuntu system, several Personal Package Archives (PPA's) can be used to install the proper package and dependencies. Note that adding these PPA's may cause conflicts with ROS.

        sudo apt-add-repository ppa:libccd-debs
        sudo apt-add-repository ppa:fcl-debs
        sudo apt-add-repository ppa:dartsim
        sudo apt-get update
        sudo apt-get install libdart-core4-dev


#### Precise

**Release Note:** in order to use DART, a full compilation of Gazebo from source is needed (as detailed in this document). The .deb packages are only shipping the ODE, Bullet, and Simbody physics engines.

Gazebo supports multiple physics engines in addition to the modified version of ODE that is used internally.

  ***Bullet Support***

  [Bullet](http://code.google.com/p/bullet/) version 2.82 is needed for Gazebo 4.0. In an Ubuntu system (precise - trusty) the OSRF repo can be used to install the proper package. Be sure to follow Step 2 in the [Ubuntu Debs section above](http://gazebosim.org/tutorials?tut=install_ubuntu&cat=install#Ubuntu) to configure your computer to accept software from packages.osrfoundation.org

        sudo apt-get update        
        sudo apt-get install libbullet2.82-dev

   ***Simbody Support***
   
   [Simbody](https://simtk.org/home/simbody/) version 3.3 is supported for Gazebo version 2.0.0 and later. In an Ubuntu system (precise - trusty) the OSRF repo can be used to install the proper package. Be sure to follow Step 2 in the [Ubuntu Debs section above](http://gazebosim.org/tutorials?tut=install_ubuntu&cat=install#Ubuntu) to configure your computer to accept software from packages.osrfoundation.org

        sudo apt-get update
        sudo apt-get install libsimbody-dev

   ***DART Support***
   
   Support for [DART](http://dartsim.github.io/) version 4.1 is integrated into Gazebo 4.0. In an Ubuntu system, several Personal Package Archives (PPA's) can be used to install the proper package and dependencies. Note that adding these PPA's may cause conflicts with ROS.

        sudo apt-add-repository ppa:libccd-debs
        sudo apt-add-repository ppa:fcl-debs
        sudo apt-add-repository ppa:dartsim
        sudo apt-get update
        sudo apt-get install libdart-core4-dev

### Optional Dependencies ###

   ***GUI test Support*** (Optional)
  
   To correctly parse the results of GUI regression tests, the xsltproc package is needed.

        sudo apt-get install xsltproc

   ***Man Page Support*** (Optional)
   
   To generate man-pages for the Gazebo executables, the ruby-ronn package is needed.

        sudo apt-get install ruby-ronn

   ***Player Support*** (Optional)

        sudo apt-get install robot-player-dev*

### Build And Install SDFormat

To install from source, you should first install the SDFormat package, then build Gazebo off of that:

1. Clone the repository into a directory:

        cd ~; hg clone https://bitbucket.org/osrf/sdformat

1. Change directory into the sdformat repository and switch to the 2.0 branch:

        cd ~/sdformat
        hg up sdf_2.0

     **Note:** the <tt>default</tt> branch is the development branch where you'll find the bleeding edge code, your cloned repository should be on this branch by default but we recommend you switch to the 2.0 branch if you desire more stability

1. Create a build directory and go there:

        mkdir build
        cd build

1. Build and install:

        cmake .. -DCMAKE_INSTALL_PREFIX=/usr
        make -j4
        sudo make install

### Build And Install Gazebo

1. Clone the repository into a directory in your home folder:

        cd ~; hg clone https://bitbucket.org/osrf/gazebo

1. Change directory in the Gazebo repository and switch to the 4.0 branch

        cd ~/gazebo
        hg up gazebo_4.0

     **Note:** the <tt>default</tt> branch is the development branch where you'll find the bleeding edge code, your cloned repository should be on this branch by default but we recommend you switch to the 4.0 branch if you desire more stability

1. Create a build directory and go there:

        mkdir build
        cd build

1. Configure Gazebo (choose either method `a` or `b` below):
   
    > a. Release mode: This will generate optimized code, but will not have debug symbols. Use this mode if you don't need to use GDB.
  
    >        cmake ../
  

    >> Note: You can use a custom install path to make it easier to switch between source and debian installs:

    >>        cmake -DCMAKE_INSTALL_PREFIX=/home/$USER/local ../
  
    > b. Debug mode: This will generate code with debug symbols. Gazebo will run slower, but you'll be able to use GDB.
  
    >        cmake -DCMAKE_BUILD_TYPE=Debug ../
    
    > Note: A big part of the compilation is the test suite. If it is useful to temporary disable it during the developemnt, you can use:

    >>        cmake ../ -DENABLE_TESTS_COMPILATION:BOOL=False
 
1. The output from `cmake ../` may generate a number of errors and warnings about missing packages. You must install the missing packages that have errors and re-run `cmake ../`. Make sure all the build errors are resolved before continuing (they should be there from the earlier step in which you installed prerequisites). Warnings alert of optional packages that are missing.

1. Make note of your install path, which is output from `cmake` and should look something like:

          -- Install path: /home/$USER/local     

1. Build Gazebo:

        make -j4

1. Install Gazebo:

        sudo make install

1. Setup environment variables

#### Local Install

If you decide to install gazebo in a local directory you'll need to modify some of your PATHs:

    echo "export LD_LIBRARY_PATH=<install_path>/local/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
    echo "export PATH=<install_path>/local/bin:$PATH" >> ~/.bashrc
    echo "export PKG_CONFIG_PATH=<install_path>/local/lib/pkgconfig:$PKG_CONFIG_PATH" >> ~/.bashrc
    source ~/.bashrc

Now try running gazebo:

    gazebo

***If Gazebo runs successfully, you're done!.***

If Gazebo was installed to `/usr/local/` and running gazebo throws an error similar to:

    gazebo: error while loading shared libraries: libgazebo_common.so.1: cannot open shared object file: No such file or directory

  , then `/usr/local/lib` is not in load path (default behavior for Ubuntu). Run the following commands and then try running gazebo again:

    echo '/usr/local/lib' | sudo tee /etc/ld.so.conf.d/gazebo.conf 
    sudo ldconfig

1. If you are interested in using Gazebo with [ROS](http://www.ros.org), see [Installing gazebo_ros_pkgs](http://gazebosim.org/tutorials?cat=connect_ros).

### Uninstalling Source-based Install

If you need to uninstall Gazebo or switch back to a debian-based install of Gazebo when you currently have installed Gazebo from source, navigate to your source code directory's build folders and run make uninstall:

    cd ~/gazebo/build
    sudo make uninstall
    cd ~/sdformat/build
    sudo make uninstall

## Compiling From Source (Mac OS X)

Gazebo and several of its dependencies can be compiled on OS X with [Homebrew](http://brew.sh) using the [osrf/simulation tap](https://github.com/osrf/homebrew-simulation). Here are the instructions:

1. Install [homebrew](http://brew.sh): `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

2. Install [XQuartz](http://xquartz.macosforge.org/landing/), which provides X11 support and is required by Gazebo and OGRE

3. For 10.8 and earlier, install [Xcode command-line tools](http://stackoverflow.com/questions/9329243/xcode-4-4-and-later-install-command-line-tools) by downloading them from Apple. For 10.9 and later, they should prompt you to install them when you install Homebrew in step 1.

4. Run the following commands:

        brew tap osrf/simulation
        brew install gazebo4
        gazebo

### Optional dependencies ###
The gazebo formula has two optional dependencies: the [Bullet](https://code.google.com/p/bullet/) and [Simbody](https://github.com/simbody/simbody) physics engines. To install with these physics engines:

        brew install gazebo4 --with-bullet --with-simbody

### Versions ###
The formula currently installs version 4.0 of gazebo.
Version 1.9 can be installed using the gazebo formula, gazebo 2.2 using gazebo2, and gazebo 3 using gazebo3.
To install the latest version of gazebo's default branch:

        brew install gazebo4 --HEAD