This site deals with compilation for Linux and similiar systems. If you're interested in a windows version, please have a look at [WindowsPort](WindowsPort.md).

# Get the Code #
```
svn checkout http://libproxy.googlecode.com/svn/trunk/ libproxy
```

# Install the Build Requirements #
Fedora:
```
yum install xulrunner-dev x11-dev xmu-dev gconf-dev dbus-dev networkmanager-dev
```

Ubuntu:
```
sudo apt-get install libmozjs-dev libxmu-dev libgconf2-dev libdbus-1-dev network-manager-dev xserver-xorg-dev
```


libproxy can be build with minimal dependencies or with several additional plugins. This matrix lists the necessary dependencies for all available plugins.

| Plugin | Fedora | Ubuntu |
|:-------|:-------|:-------|
| NetworkManager Plugin| dbus-devel, NetworkManager-devel | libdbus-1-dev, network-manager-dev |
| Gnome Config Plugin | GConf2-devel | libgconf2-dev |
| KDE Config Plugin | kdelibs-devel | kdelibs5-dev |
| PAC via Firefox/Xulrunner | xulrunner-devel | firefox-dev or libmozjs-dev |
| PAC via WebKit | webkitgtk-devel | libwebkit-dev |

Information on libproxy bindings coming soon...


# Building the Code #

For the most up-to-date information about building and configuring libproxy please refer to the [INSTALL](http://code.google.com/p/libproxy/source/browse/trunk/INSTALL) file.

## Quick Start ##
```
1. mkdir build
2. cd build
3. cmake ..
4. make
5. make install
```

## Testing it Out ##
```
/usr/local/bin/proxy http://www.google.com
```
