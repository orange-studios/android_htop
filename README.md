# android_htop
交叉编译htop for android

源代码版本：
htop-2.2.0
ncurses-6.1

编译之前确认已下载好ndk，下载下来解压一下就能用。

先编译ncurses
`
./configure CC=/home/orangeyang/android-ndk-r15c/tmp/compile-tools/bin/aarch64-linux-android-gcc --prefix=/home/orangeyang/htop/ncurses-6.0/ncinstall --host=arm-linux-androideabi --with-shared --without-progs CPPFLAGS=-I/home/orangeyang/android-ndk-r15c/sysroot/usr/include
make
make install
cd /home/orangeyang/htop/ncurses-6.0/ncinstall/lib
ln -s libncurses.so.6 libncurses6.so
`

再编译htop
`
./configure CC=/home/oragneyang/android-ndk-r15c/tmp/compile-tools/bin/aarch64-linux-android-gcc --prefix=/home/orangeyang/htop/htop-2.0.2/htinstall --host=arm-linux-androideabi --bindir=/home/orangeyang/htop/htop-2.0.2/htinstall/xbin --with-sysroot=/home/orangeyang/android-ndk-r15c/sysroot CFLAGS="-I/home/orangeyang/htop/ncurses-6.0/htinstall/include -I/home/orangeyang/htop/ncurses-6.0/htinstall/include/ncurses -I/home/orangeyang/android-ndk-r15c/sources/android/support/include -pie -fPIE" LDFLAGS=-L/home/orangeyang/htop/ncurses-6.0/_install/lib --disable-unicode
make
make install

注: error: Android 5.0 and later only support position-independent executables (-fPIE).
如果报这个错是因为PIE这个安全机制从4.1引入，但是Android L之前的系统版本并不会去检验可执行文件是否基于PIE编译出的。因此不会报错。但是Android L已经开启验证，如果调用的可执行文件不是基于PIE方式编译的，则无法运行。解决办法非常简单，在flag加上-pie -fPIE即可。
`

将文件夹ncinstall和文件夹htinstall拷贝到android设备的/data目录
在/data/htinstall/xbin目录执行
`
chmod a+x htop
export TERM=xterm
export TERMINFO=/data/ncinstall/share/terminfo
export LD_LIBRARY_PATH=/data/ncinstall/lib
./htop

注：
Error opening terminal: xterm
设置TERM、TERMINFO即可
`
