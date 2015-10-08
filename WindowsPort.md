# Introduction #

libproxy 0.4 (not yet released) now contains a native Windows port.  This port is targeted towards application developers who will want to ship libproxy bundled with their application.

# What's working #

  * It builds and links
  * Applications can link against it
  * We read configuration from the windows registry (ie Internet Options)

# What's not working #
  * PAC / WPAD: this is due to not yet building against mozjs or webkit. TODO
  * Threading: this isn't a problem, just know that libproxy is not threadsafe on win32

# Requirements #
  * Visual C++ (any version)
  * CMake >= 2.8
  * Subversion

# Compilation #
Check out the latest trunk using SVN.  Use CMake to generate the build files.  The libproxy.sln file should be in your build directory.  Double click it to load the project in Visual Studio.  Build like normal.