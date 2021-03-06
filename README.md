# pstree
Add a system call “pstree” in Android OS

***********source**********

1. JDK

  http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
  
2. SDK
  http://www.cs.sjtu.edu.cn/~fwu/teaching/res/androidsdk-linux.tar.gz

3. NDK
  http://www.cs.sjtu.edu.cn/~fwu/teaching/res/androidndk-r11-linux-x86_64.zip

  extract into ~/android or /usr/lib/android/
  
  type ndk-build -v to check
  
4. android shell
	http://www.cs.sjtu.edu.cn/~fwu/teaching/res/androidkernel.tar.gz

	extract into the usr folder


**********steps**************

1. Modify the environment variables in ~/.bashrc

  export PATH=${PATH}:/home/lanouyu/Downloads/android-sdk-linux/tools
  
  export PATH=${PATH}:/home/lanouyu/Downloads/android-sdk-linux/platform-tools

  export JAVA_HOME=/usr/lib/jdk1.8.0_73
  
  export JRE_HOME=/usr/lib/jdk1.8.0_73/jre
  
  export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
  
  export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
  
  export PATH=${PATH}:~/android
  
  export PATH=${PATH}:/home/lanouyu/Downloads/android-ndk-linux/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin
2. Start AVD with the new shell

  please install 32-bit lib first:
  
  sudo apt-get install libc6:i386 libgcc1:i386 gcc-4.6-base:i386 libstdc++5:i386 libstdc++6:i386
  
  sudo apt-get install lib32stdc++6

  then start it:

  ~/Downloads/android-sdk-linux/tools/emulator -avd OsPrj-5140309001 -kernel ~/kernel/goldfish/arch/arm/boot/zImage -show-kernel
  
  this process maybe very slow, please wait patiently.
  
  to check AVD status: adb devices

3. type make in ptree folder, then we get a file ptree.ko, which is our module

  make

4. upload ptree.ko file to AVD
 
  adb push ptree.ko /data/misc

5. go to adb shell and install mod

  adb shell
  
  cd data/misc
  
  insmod ptree.ko
  
  lsmod (to check)

6. test
  cd ../test/jni

  ndk-build 
  (The executable file is in test/libs/armeabi)
  
  adb push ../libs/armeabi/ptree /data/misc
  
  ./ptree
  
  ******************OK**************
