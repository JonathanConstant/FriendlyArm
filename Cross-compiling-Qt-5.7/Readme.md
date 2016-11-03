  Here are the steps for cross-compiling Qt 5.7 for the NanoPi M3 (it would probably work with other FriendlyArm boards)
  
  On the board : 
    
    **Step** 1 : 
      - Because libts-dev is not in the debian packages, we add the wheezy distribution for apt-get 
      Add the following line at the end of the file sources.list :
      
      sudo nano /etc/apt/sources.list
      deb http://ftp.cn.debian.org/debian wheezy main non free contrib
      deb-src http://ftp.cn.debian.org/debian wheezy main non free contrib
      
      
    **Step 2 :** 
  
    
  
  
