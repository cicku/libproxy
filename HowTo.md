# Programming #
## C(++) ##
### API ###
There are only three functions in the external API:
```
/**
 * Creates a new pxProxyFactory instance. This instance should be kept
 * around as long as possible as it contains cached data to increase
 * performance.  Memory usage should be minimal (cache is small) and the
 * cache lifespan is handled automatically.
 *
 * @return A new pxProxyFactory instance or NULL on error
 */
pxProxyFactory *px_proxy_factory_new(void);

/**
 * Get which proxies to use for the specified URL.
 *
 * A NULL-terminated array of proxy strings is returned.
 * If the first proxy fails, the second should be tried, etc...
 * Don't forget to free the strings/array when you are done.
 * If an unrecoverable error occurs, this function returns NULL.
 *
 * Regarding performance: this method always blocks and may be called
 * in a separate thread (is thread-safe).  In most cases, the time
 * required to complete this function call is simply the time required
 * to read the configuration (i.e. from gconf, kconfig, etc).
 *
 * In the case of PAC, if no valid PAC is found in the cache (i.e.
 * configuration has changed, cache is invalid, etc), the PAC file is
 * downloaded and inserted into the cache. This is the most expensive
 * operation as the PAC is retrieved over the network. Once a PAC exists
 * in the cache, it is merely a javascript invocation to evaluate the PAC.
 * One should note that DNS can be called from within a PAC during
 * javascript invocation.
 *
 * In the case of WPAD, WPAD is used to automatically locate a PAC on the
 * network.  Currently, we only use DNS for this, but other methods may
 * be implemented in the future.  Once the PAC is located, normal PAC
 * performance (described above) applies.
 *
 * The format of the returned proxy strings are as follows:
 *   - http://[username:password@]proxy:port
 *   - socks://[username:password@]proxy:port
 *   - socks5://[username:password@]proxy:port
 *   - socks4://[username:password@]proxy:port
 *   - <procotol>://[username:password@]proxy:port
 *   - direct://
 * Please note that the username and password in the above URLs are optional
 * and should be use to authenticate the connection if present.
 *
 * For SOCKS proxies, when the protocol version is specified (socks4:// or
 * sock5://), it is expected that only this version is used. When only
 * socks:// is set, the client MUST try SOCKS version 5 protocol and, on
 * connection failure, fallback to SOCKS version 4.
 *
 * Other proxying protocols may exist. It is expected that the returned
 * configuration scheme shall match the network service name of the
 * proxy protocol or the service name of the protocol being proxied if the
 * previous does not exist. As an example, on Mac OS X you can configure a
 * RTSP streaming proxy. The expected returned configuration would be:
 *   - rtsp://[username:password@]proxy:port
 * 
 * @url The URL we are trying to reach
 * @return A NULL-terminated array of proxy strings to use
 */
char **px_proxy_factory_get_proxies(pxProxyFactory *self, const char *url);

/**
 * Frees the pxProxyFactory instance when no longer used.
 */
void px_proxy_factory_free(pxProxyFactory *self);
```

### Example ###
Take a look at the simple pseudo-code below to get started.  A complete functioning program is found in the source tarball at src/bin/proxy.c.

```
// This contains the libproxy public API
#include <proxy.h>

// A fake function that fetches the URL using the specified proxy.
// This function is just a pseudo-summary of "whatever your application does"
// Returns 1 on successful GET request.
extern int fetch_url(char *url, char *proxy);

int main()
{
  // Create a proxy factory instance
  pxProxyFactory *pf = px_proxy_factory_new();
  if (!pf) return 1;

  // Get which proxies to use in order to fetch "http://www.google.com"
  char **proxies = px_proxy_factory_get_proxies(pf, "http://www.google.com");

  // Iterate over the returned proxies, attemping to fetch the URL
  for (int i=0 ; proxies[i] ; i++)
    if (fetch_url("http://www.google.com", proxies[i]))
      break;

  // Free the proxy list
  for (int i=0 ; proxies[i] ; i++)
    free(proxies[i]);
  free(proxies);

  // Free the proxy factory
  px_proxy_factory_free(pf);

  return 0;
}
```

## Python ##

```
import libproxy

URL = "http://www.google.com"

pf = libproxy.ProxyFactory()
for proxy in pf.getProxies(URL):
  # Do something with the proxy
  if fetch_url(URL, proxy):
    break
```

# Controlling libproxy #
## Module System ##
**DISCLAIMER: The following has not been declared stable and may change or go away**

In order to provide a wide variety of functionality while keeping the core of libproxy small, we use a module system.  Most of our functionality is provided directly from the backend modules.  There are 5 types of modules, each which provide a certain function:
  1. config - reads configuration
  1. ignore - determines whether or not a given ignore pattern matches the current URL
  1. network - determines whether or not the network topology has changed
  1. pacrunner - evaluates a PAC file in a given javascript interpreter
  1. wpad - implements the "guts" of the wpad detection

All modules will be loaded at px\_proxy\_factory\_new() and will remain loaded until px\_proxy\_factory\_free() is called.  There are two exceptions to this.  The first is if the plugin type is declared as a singleton.  This is only true in the case of pacrunners and allows us to have only the first javascript engine loaded in memory.  The second case is a SESSION config module (discussed below).  SESSION config modules MUST NOT allow instantiation if the application is not being loaded in the correct session.  Thus, if you are running in GNOME, the KDE SESSION config module will not be loaded.

### Blaclist/Whitelist ###
**ATTENTION! USE THE FOLLOWING FEATURE WITH GREAT CARE! DO NOT USE IT FOR CONFIG MODULES!**

If you want to blacklist certain modules you can use the PX\_MODULE\_BLACKLIST and/or PX\_MODULE\_WHITELIST environmental variables.  For instance, if you are using libproxy in a webkit based browser, you will probably want to force the usage of the webkit pacrunner.  This is done as follows:
```
export PX_MODULE_BLACKLIST=pacrunner_*
export PX_MODULE_WHITELIST=pacrunner_webkit
```

This functionality exists primarily for our internal testing and for the web-browser case I listed above.  For config modules use the PX\_CONFIG\_ORDER functionality described below.

### Config Modules ###
Config modules read proxy configuration from a source. Every config module has a type.  There are three confnig types:
  1. SYSTEM  - Defines configuration on a system-wide basis
  1. USER    - Defines configuration on a user-wide basis
  1. SESSION - Defines configuration for this current login session only
A module can also choose to have no config type.

The config module sources (as of 0.3.0) are:
  1. direct - always return "direct://", this is the global fallback
  1. envvar - reads http\_proxy, https\_proxy, ftp\_proxy and no\_proxy environment variables
  1. file - reads /etc/proxy.conf (SYSTEM) and/or ~/.proxy.conf (USER)
  1. gnome - reads gconf (SESSION)
  1. kde - reads kconfig (SESSION)
  1. wpad - always returns "wpad://", this is used to fall back on autodetect if desired

By default, modules are called in the following order:
  1. USER
  1. SESSION
  1. SYSTEM
  1. envvar
  1. wpad
  1. direct

The order _within_ the categories is undefined and could be random. If a module says it can't find the configuration, the next module is tried.

The module order can be manually specified through an environmental variable (PX\_CONFIG\_ORDER).  The order indicated in this variable **does not replace** the internal order, but prepends to it.  Thus, if you wanted to prefer (for forcing a module, see whitelist/blacklist above) the usage of envvar, you would specify the following which would put envvar first in the list:
```
export PX_CONFIG_ORDER=config_envvar
```

You can also lock-down the module order for all users on a system using /etc/proxy.conf.  This feature is not yet documented (any volunteers!?).