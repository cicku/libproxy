Use the following template when contacting a project or distribution to let them know about libproxy.  Please replace BUG\_URL and NAME with the url to the bug report filed and your name, respectively.

### BEGIN\_TEMPLATE ###
Hello everyone!

I would like to announce the release of version 0.2 of libproxy, an
open-source library that provides functions to manage proxy
configurations uniformly across applications and user sessions.
libproxy has a stable API and modules to read configuration from
GNOME, KDE, a file, or an environment variable (for compatibility with
existing implementations). In addition, libproxy automatically adjusts
for changing network topology via integration with `NetworkManager` and
is able to autodetect proxy settings through WPAD and PAC compliance.

Through these features, an application that links to libproxy will not
have to worry about proxy configuration or even a change of networks:
libproxy will automatically provide the correct proxy to use.

Please have a look at http://libproxy.googlecode.com for more info. A
bug requesting libproxy adoption has also been filed: BUG\_URL

Thanks for your time!
NAME
### END\_TEMPLATE ###