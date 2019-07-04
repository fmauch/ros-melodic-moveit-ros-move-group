# Maintainer: Timon Engelke <aur@timonengelke.de>
pkgdesc="ROS - The move_group node for MoveIt."
url='https://moveit.ros.org'

pkgname='ros-melodic-moveit-ros-move-group'
pkgver='0.10.8'
_pkgver_patch=0
arch=('any')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-melodic-moveit-core
  ros-melodic-tf
  ros-melodic-pluginlib
  ros-melodic-moveit-ros-planning
  ros-melodic-std-srvs
  ros-melodic-actionlib
  ros-melodic-catkin)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]})

ros_depends=(ros-melodic-moveit-core
  ros-melodic-tf
  ros-melodic-pluginlib
  ros-melodic-moveit-kinematics
  ros-melodic-moveit-ros-planning
  ros-melodic-std-srvs
  ros-melodic-actionlib)
depends=(${ros_depends[@]})

# Git version (e.g. for debugging)
# _tag=release/melodic/moveit_ros_move_group/${pkgver}-${_pkgver_patch}
# _dir=${pkgname}
# source=("${_dir}"::"git+https://github.com/ros-gbp/moveit-release.git"#tag=${_tag})
# sha256sums=('7507264909f49f795664a5443110363bcca00cbcc3380d421af9a586d5b0ede2')

# Tarball version (faster download)
_dir="moveit-release-release-melodic-moveit_ros_move_group-${pkgver}-${_pkgver_patch}"
source=("${pkgname}-${pkgver}-${_pkgver_patch}.tar.gz"::"https://github.com/ros-gbp/moveit-release/archive/release/melodic/moveit_ros_move_group/${pkgver}-${_pkgver_patch}.tar.gz")
sha256sums=('241e9b5170d5f679e7603cb45a5959614a70c91ffa77591b5d111d7b7930f8b6')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python3.7m \
        -DPYTHON_LIBRARY=/usr/lib/libpython3.7m.so \
        -DPYTHON_BASENAME=-python3.7m \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
