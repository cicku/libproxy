# Introduction #

Instructions to make a libproxy release.


# Details #

  * Update NEWS file and version number in libproxy/CMakeLists.txt
  * Commit those changes with comment "Version X.X.X"
  * svn commit -m "Version X.X.X"
  * Create a version tag
  * svn copy https://libproxy.googlecode.com/svn/trunk https://libproxy.googlecode.com/svn/tags/libproxy-X.X.X -m "Version X.X.X"
  * Create source packages
  * cmake . && make package\_source
  * Upload the ZIP and Tar.gz in the download section with lables Type-Source, OpSys-All and Featured
  * Removed featured tag from previous files
  * Send announcement email to libproxy@googlegroups.com