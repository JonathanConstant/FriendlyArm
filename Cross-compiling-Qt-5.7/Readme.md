Here are the steps for cross-compiling Qt 5.7 for the NanoPi M3 (it would probably work with other FriendlyArm boards) 

###Step 1 : Configure the board 
Because libts-dev is not in the debian packages, we add the wheezy distribution for apt-get 
Add the following lines at the end of the file sources.list :       
> sudo nano /etc/apt/sources.list  
> deb http://ftp.cn.debian.org/debian wheezy main non free contrib  
> deb-src http://ftp.cn.debian.org/debian wheezy main non free contrib 

Install some libraries : 
> sudo apt-get update  
> sudo apt-get build-dep qt4-x11  
> sudo apt-get build-dep libqt5gui5  
> sudo apt-get install libudev-dev libinput-dev libts-dev libxcb-xinerama0-dev libxcb-xinerama0  
> sudo apt-get install libglib2.0-0 
> sudo apt-get install libbluetooth-dev   
> sudo apt-get install libdbus-1-dev libdbus-c++-dev  

###Step 2 : Configure the host 
(maybe you will have to install some others libraries in your host, message will appear during the configure or the make)  

Download the cross-compiler, I took the cross compiler of the raspberry _gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-_  
Download Qt 5.7 with the sources  
Create the sysroot directory and synchronize the files with the board, replace IP with yours:    

> mkdir sysroot sysroot/usr     
> rsync -avz fa@IP:/lib sysroot  
> rsync -avz fa@IP:/usr/include sysroot/usr  
> rsync -avz fa@IP:/usr/lib sysroot/usr  
> rsync -avz fa@IP:/usr/nexell sysroot/usr  

Download the device config file linux-nanopi-m3-g++ and copy it in your Qt directory Qt5.7/5.7/Src/qtbase/mkspecs/devices

To use OpenGL we need to do some modifications in Qt srouces :
> nano Qt-5.7.0/5.7/Src/qtbase/src/plugins/platforms/eglfs/deviceintegration/eglfs_mali/qeglfsmaliintegration.cpp

Change every "fbdev_window" in "fbdev_window2"

###Step3 : Compile Qt
Go to Qt5.7/5.7/Src and execute the following command, think to change the path with yours (I don't install qt3d and qtwayland modules):
> ./configure -release -opengl es2 -device linux-nanopi-m3-g++ -device-option CROSS_COMPILE=/home/jonathan/NanoPIM3/cross_compile/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf- -sysroot /home/jonathan/NanoPIM3/cross_compile/sysroot -opensource -confirm-license -make libs -prefix /usr/local/qt5pi -extprefix /home/jonathan/NanoPIM3/cross_compile/qt5pi -hostprefix /home/jonathan/NanoPIM3/cross_compile/qt5 -skip qt3d -skip qtwayland -I /home/jonathan/NanoPIM3/cross_compile/sysroot/usr/nexell/include/khronos -L /home/jonathan/NanoPIM3/cross_compile/sysroot/usr/nexell/lib -v  

> make  
> sudo make install  

Synchronize Qt with your board :  
> rsync -avz qt5pi fa@IP:/usr/local  

Install the debugger on the board :
> sudo apt-get install gdbserver

Install the debugger on the host :
> sudo apt-get install gdb-multiarch 

Configure Qt Creator with your board, the cross compiler and the debugger and if all is fine, Qt must work on your NanoPi M3 ! 
Enjoy :)




  
  
