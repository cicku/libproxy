libproxy is a library that provides automatic proxy configuration management.

# The Problem #

**Problem statement**: _Applications suck at correctly and consistently supporting proxy configuration._

Proxy configuration is problematic for a number of reasons:
  1. There are a variety of places to get configuration information
  1. There are a variety of proxy types
  1. Proxy auto-configuration (PAC) requires Javascript (which most applications don't have)
  1. Automatically determining PAC location requires an implementation of the WPAD protocol

These issues make programming with support for proxies _hard_.  Application developers only want to answer the question: **Given a network resource, how do I reach it?**
Because this is their concern, most applications just give up and try to read the proxy from an environment variable.  This is problematic because:
  1. Given the increased use of mobile computing, network switching frequently occurs during the lifetime of an application
  1. Each application is required to implement the (non-trivial) WPAD and PAC protocols, including a Javascript engine
  1. It prevents a network administrator from locking down settings on a particular host or user
  1. In most cases, the environmental variable is almost never correct by default

# The Solution #
libproxy exists to answer the question: **Given a network resource, how do I reach it?** It handles all the details, enabling you to get back to programming.

GNOME? KDE? Command line? WPAD? PAC? Network changed? It doesn't matter! Just [ask libproxy what proxy to use](HowTo.md): you get simple code and your users get correct, consistant behavior and broad infrastructure compatibility.

# Why use libproxy? #
libproxy offers the following features:
  * support for all major platforms: Windows, Mac and Linux/UNIX (see upcoming 0.4 release)
  * extremely small core footprint
  * no external dependencies within libproxy core (libproxy plugins may have dependencies)
  * only 3 functions in the stable-ish external API (1.0 will offer full stability)
  * dynamic adjustment to changing network topology
  * a standard way of dealing with proxy settings across all scenarios
  * a sublime sense of joy and accomplishment

# Frequently Asked Questions #
  * [What features does libproxy currently provide? Future plans?](Features.md)
  * [Does libproxy support proxy authentication?](Features.md)
  * [Which plugins get used in which order?](Features.md)
  * [How do I use libproxy in my application?](HowTo.md)
  * [What programs currently use libproxy?](Applications.md)
  * [How can I help?](GetInvolved.md)
  * [How can I donate?](https://www.paypal.com/cgi-bin/webscr?cmd=_xclick&business=nathaniel%40natemccallum%2ecom&item_name=libproxy&no_shipping=1&cn=Thanks&tax=0&currency_code=USD&lc=US&bn=PP%2dDonationsBF&charset=UTF%2d8)
  * [How can I contact you?](Contact.md)