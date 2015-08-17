# Maintainer: Paul Hollensen < ${lastname::8} @cs.dal.ca >
pkgname=ros-core
pkgver=1.8.9
pkgrel=3
pkgdesc="Robot Operating System - Fuerte - ROS-Full variant of core libraries"
url="http://www.ros.org"
arch=('x86_64' 'i686')
license=('BSD')
depends=('python2-empy' 'python2-nose' 'log4cxx' 'python2-rospkg' 'rosinstall' 'vcstools' 'wxpython'
         'boost' 'swig' 'qt')
optdepends=('python2-rosdep')
makedepends=()
source=('http://ros.org/rosinstalls/fuerte-ros-full.rosinstall'
        'rosbag_recorder_boost150_TIME_UTC_.patch'
        'rostime_boost150_system.patch'
        'roslisp-extras.patch')
md5sums=('59faab4fee1567dd0f6f2f373673bb91'
         '443e63dd76e1e796382b6aed52000e3c'
         'd575e1c211d1389e634f63afba09c326'
         '60325a3823752febdaff313d00d26d1a')

build() {
  mkdir -p "$srcdir/ros-underlay" && cd "$srcdir/ros-underlay"
  wget http://ros.org/rosinstalls/fuerte-ros-full.rosinstall -O .rosinstall
  rosinstall --catkin .
  patch ros_comm/clients/roslisp/cmake/roslisp-extras.cmake.in ../roslisp-extras.patch
  patch ros_comm/tools/rosbag/src/recorder.cpp ../rosbag_recorder_boost150_TIME_UTC_.patch
  patch roscpp_core/rostime/CMakeLists.txt ../rostime_boost150_system.patch
  for file in $(grep -rl 'env python *$' .); do sed -i 's/env python *$/env python2/g' $file; done
  mkdir build && cd build
  cmake -DCMAKE_INSTALL_PREFIX=/opt/ros/fuerte \
	-DSETUPTOOLS_DEB_LAYOUT=OFF \
	-DPYTHON_EXECUTABLE=/usr/bin/python2 \
	-DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
	-DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
	..
  make -j8
}

package() {
  cd "${srcdir}/ros-underlay/build"
  make DESTDIR="${pkgdir}" install
}
