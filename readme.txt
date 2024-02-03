# on ubuntu 16.04 and Raspberry Pi OS based on Debian 12 the following is required too
# apt-get install libxml-parser-perl
#
# based on http://davidbolton.info/articles/build_linuxsampler.html
apt-get install subversion
mkdir lssvn
cd lssvn
svn co https://svn.linuxsampler.org/svn/libgig/trunk libgig
svn co https://svn.linuxsampler.org/svn/linuxsampler/trunk linuxsampler
apt-get install g++ debhelper pkg-config automake libtool fakeroot libsndfile1-dev doxygen uuid-dev lv2-dev
cd libgig
dpkg-buildpackage -rfakeroot -b
cd ..
dpkg -i gigtools_4.1.0-1_amd64.deb libgig8_4.1.0-1_amd64.deb libgig-dev_4.1.0-1_amd64.deb
(arm: dpkg -i gigtools_4.1.0-1_armhf.deb libgig8_4.1.0-1_armhf.deb libgig-dev_4.1.0-1_armhf.deb)
apt-get install bison libjack-jackd2-dev flex libsqlite3-dev dssi-dev
cd linuxsampler/
(arm:
  # if building for ARM architecture, choose the appropriate patch and apply it
  svn patch ../../linuxsampler-aarch64.patch .
)
dpkg-buildpackage -rfakeroot -b
cd ..
# important: the linuxsampler package must be installed last since it depends on liblinuxsampler
dpkg -i liblinuxsampler_2.1.0-1_amd64.deb liblinuxsampler-dev_2.1.0-1_amd64.deb linuxsampler_2.1.0-1_amd64.deb
(arm: dpkg -i liblinuxsampler_2.1.0-1_armhf.deb liblinuxsampler-dev_2.1.0-1_armhf.deb linuxsampler_2.1.0-1_armhf.deb)
