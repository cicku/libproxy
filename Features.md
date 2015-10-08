# Features #
| **Area** | **Version Added** | **Version Stablized** | **Dependencies** |
|:---------|:------------------|:----------------------|:-----------------|
| _**API**_ | | | |
| Thread Safety | 0.2.2             |                       |                  |
| External C API | 0.1               | 0.2                   |                  |
| External Python API | 0.2               |                       |                  |
| External .NET API | 0.2               |                       |                  |
| Internal API | 0.1               |                       |                  |
| _**Core Features**_ | | | |
| CLI Tool | 0.1               |                       |                  |
| [Proxy Auto-Configuration (PAC)](http://en.wikipedia.org/wiki/Proxy_auto-config) | 0.1               |                       |                  |
| [Web Proxy Auto-Discovery (WPAD)](http://en.wikipedia.org/wiki/Web_Proxy_Autodiscovery_Protocol) | 0.1               |                       |                  |
| Configuration Lockdown | 0.2               |                       |                  |
| _**Configuration Plugins**_ | | | | **Config Type**   |
| Environment variable config plugin | 0.1               | 0.2                   |                  | No type specified |
| INI-style file config plugin | 0.2               |                       |                  | USER & SYSTEM |
| GNOME config plugin | 0.1               |                       | x11, xmu, gconf  | SESSION |
| KDE config plugin | 0.2               |                       | x11, xmu         | SESSION |
| Windows registry config plugin |                   |                       |                  |
| Mac OS X InternetConfig config plugin |                   |                       |                  |
| LDAP config plugin |                   |                       |                  |
| [dconf](http://live.gnome.org/dconf) config plugin |                   |                       |                  |
| _**Javascript Plugins**_ | | | |
| PAC via Firefox/Xulrunner | 0.1               |                       | firefox or xulrunner |
| PAC via WebKit | 0.2.3             |                       | WebKit or JavaScriptCore |
| PAC via Safari |                   |                       |                  |
| PAC via Internet Explorer |                   |                       |                  |
| _**Other Plugins**_ | | | |
| NetworkManager plugin | 0.2               |                       | dbus             |

See BuildingFromSource for specific build dependencies and distro package requirements.

# Proxy Authentication #
Because proxy authentication is protocol specific, it is outside the scope of this library.  libproxy tells you WHICH proxy servers to try to use, not HOW to use them.