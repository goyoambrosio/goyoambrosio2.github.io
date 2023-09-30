---
title: "Preventing DNS leaks in VPNs"
layout: # single
classes: wide
author_profile: # true
read_time: # true
comments: # true
share: # true
related: #true
excerpt: # Auto-generated page excerpt is added to the overlay text or can be overridden here.
excerpt_separator: # "<!--more-->"
header:
  overlay_color: "#000000" 
  overlay_filter: "0.6" # 1.0 Darkest
  overlay_image: "/assets/images/2018/06/DNS.jpeg"
  caption: # "Photo credit: [**Avira Blog**](https://blog.avira.com/dont-let-your-dns-leak/)"
  image_description: "DNS leaks"
  teaser: "/assets/images/2018/06/DNS.jpeg"
  show_overlay_excerpt: # true
categories:
  - tutorial
tags: 
  - DNS
  - leak
  - VPN
  - Ubuntu
  - systemd.resolved
  - NetworkManager
  - resolution
  - anonymity
  - lookup
  - D-Bus
  - resolv.conf
last_modified_at: 2018-06-21
---

There are different ways to fix DNS leaks depending on your initial system
configuration. A common situation, as occurs in Ubuntu 18.04, is when your
system uses `systemd-resolved` as the network name resolution service.

## The problem ##

A virtual private network (VPN) extends a private network across a public
network, and enables users to send and receive data across shared or public
networks as if their computing devices were directly connected to the private
network. Applications running across a VPN may therefore benefit from the
functionality, security, and management of the private network.[^1]

<img style="display: block;max-width: 100%;height: auto;margin-left: auto;margin-right: auto;border-radius: 8px;background-color: lightgray;" src="https://blog.emsisoft.com/wp-content/uploads/2017/05/how_a_vpn_works_infographic-730x484.png" alt="How a VPN works">

Originally VPN were just a way to connect business networks together securely
over the internet or allow you to access a business network from home. But
nowadays are really popular for other reasons. As a VPN allows you to create a
secure connection to another network over the Internet, it can also be used to
access region-restricted websites, shield your browsing activity from prying
eyes on public Wi-Fi, and more.[^2]

A common consumer-oriented scenario is to contract a VPN service with a
principle to follow in mind. The VPN user expects that all traffic should go
through the vpn service, i.e. every single outgoing ip package should be
relayed to the vpn server.

But, if you have reached this post is because you have noticed that when your
system makes DNS lookups it choses to ignore the fact that a VPN connection is
in use and instead relays those requests to other DNS servers, perhaps owned by
your ISP, your company or even a local server destroying your anonymity. That’s
called a DNS leak.

<img  style="display: block;max-width: 100%;height: auto;margin-left: auto;margin-right: auto;border-radius: 8px;background-color: lightgray;" src="https://xvp.akamaized.net/assets/customer_tools/dns_leak/dns-leak-5f52236d7c3f0764fb719087b5ef02c1.png" alt="DNS Leak">

A DNS leak is a problem with the network configuration that results in loss of
privacy by sending DNS queries over insecure links instead of using the VPN
connection. Websites exist to allow testing to determine whether a DNS leak is
occurring, including [dnsleaktest](https://www.dnsleaktest.com),
[ExpressVPN](https://www.expressvpn.com/dns-leak-test), etc.

## systemd-resolved ##

`systemd-resolved` is a system service that provides network name resolution to
local applications. It implements a caching and validating DNS/DNSSEC stub
resolver, as well as an LLMNR resolver and responder.[^3]

Local applications may submit network name resolution requests via three interfaces: 

- The native, fully-featured API systemd-resolved exposes on the bus.
- The glibc `getaddrinfo` API. This API is widely supported, including beyond the Linux platform.
- A local DNS stub listener on IP address 127.0.0.53 on the local loopback
  interface. Programs issuing DNS requests directly, bypassing any local API may
  be directed to this stub, in order to connect them to `systemd-resolved`.

From a long time ago, file `resolv.conf` specifies the name servers for DNS
lookups. This file is used in various operating systems to configure the
system's Domain Name System (DNS) resolver. The file is a plain-text file
usually created by the network administrator or by applications that manage the
configuration tasks of the system.[^4]

The `resolv.conf` configuration file contains information that determines the
operational parameters of the DNS resolver and is usually located in the `/etc`
directory of the file system. The file is either maintained manually or by
applications or services that override its content. That's what happens with
`systemd-resolved.`

`systemd-resolved` has four modes of handling `/etc/resolv.conf`:

- Maintaining the `/run/systemd/resolve/stub-resolv.conf` file for compatibility
  with traditional Linux programs. This file lists the 127.0.0.53 DNS stub as
  the only DNS server and may be symlinked from `/etc/resolv.conf`. It also
  contains a list of search domains that are in use by systemd-resolved. The
  list of search domains is always kept up-to-date. **This mode of operation is
  recommended** and the default in Ubuntu 18.04 Desktop.
- Providing a static file `/usr/lib/systemd/resolv.conf` that lists the
  127.0.0.53 DNS stub as only DNS server. This file may be symlinked from
  `/etc/resolv.conf` in order to connect all local clients that bypass local DNS
  APIs to `systemd-resolved`. This file does not contain any search domains.
- Maintaining the `/run/systemd/resolve/resolv.conf` file for compatibility with
  traditional Linux programs. This file may be symlinked from `/etc/resolv.conf`
  and is always kept up-to-date, containing information about all known DNS
  servers. In this mode local clients that bypass any local DNS API will also
  bypass `systemd-resolved` and will talk directly to the known DNS servers.
  -Alternatively, `/etc/resolv.conf` may be managed by other packages, in which
  case `systemd-resolved` will read it for DNS configuration data. In this mode
  of operation `systemd-resolved` is consumer rather than provider of this
  configuration file.
  
To use a system DNS server preferably for all domains and thus avoid DNS leaks,
you must set `DNS Domain: ~.`[^6]. The construct `~.` is composed of `~` to indicate
a routing domain and `.` to indicate the DNS root domain that is the implied
suffix of all DNS domains. To do so you must not write over `/etc/resolv.conf`,
instead instruct `systemd-resolved` setting the DNS Domain property.

`systemd-resolved` may be used via two interfaces: directly via its D-Bus
interface, or via glibc NSS `getaddrinfo()`, in which case it provides forward
and reverse hostname resolution. The former is useful to retrieve arbitrary DNS
resource records or DNSSEC authentication information. It generally provides a
more fine-grained control over the lookups made than the latter. In addition it
provides calls to introspect and configure the DNS resolver.[^7]

### D-Bus ###

D-Bus is an Inter-Process Communication (IPC) mechanism, which is generally used
to implement cross-host communication based on local IPC based on AF_UNIX
sockets.[^8]

D-Bus adopts the idea of ​​RPC (Remote Procedure Calling), which is equivalent to
the local RPC. The RPC framework itself is responsible for the preparation of
messages, decoding, security verification, etc. These will greatly simplify the
development of applications.

D-Bus protocol supports both RPC and publish-subscribe mechanisms. It contains
some basic concepts[^9]:

- A logical **Bus** over which connected applications may communicate.
  Applications connected to the bus may query for the availability of objects,
  call remote methods on them, and request notification for the signals they
  emit.
- **Service**. A *service* is a program that provides IPC APIs. Each service has a
  reverse domain name structured identifier name. For example
  `org.freedesktop.resolve1` corresponds to the service `systemd-resolved`.
- **Object**. An *object* is equivalent to the address of the communication. Objects
  are identified within an application via their *object path*. The *object
  path* intentionally looks just like a standard Unix file system path. The
  primary difference is that the path may contain only numbers, letters,
  underscores, and the `/` character. For example, `/org/freedesktop/resolve1`
  is the object path of an `org.freedesktop.resolve1`'s service object.
- **Interface**. The *interfaces* define the *methods* and *signals* supported
  by D-Bus objects. Each object contains one or more interfaces. The naming
  convention for D-Bus interfaces is similar to that of well-known bus names. To
  reduce the chance of name clashes, the accepted convention is to prefix the
  interface name with a reversed DNS domain name. For example, the standard
  *Introspection* interface is `org.freedesktop.DBus.Introspectable`.
- **Method** and **Signals**. D-Bus methods may accept any number of arguments
  and may return any number of values, including none. Each method and signal
  explicitly defines the number and types of arguments they accept. These are
  encoded as D-Bus Signature strings. Methods and signals must begin with a
  letter and may consist of only letters, numbers, and underscores.
- **Signature Strings**. D-Bus uses a string-based type encoding mechanism
  called *Signatures* to describe the number and types of arguments requried by
  methods and signals. Signatures are used for interface
  declaration/documentation, data marshalling, and validity checking.

<img style="display: block;max-width: 100%;height: auto;margin-left: auto;margin-right: auto;border-radius: 8px;background-color: lightgray;" src="https://dbus.freedesktop.org/doc/diagram.svg" alt="DBus diagram">
  
To interact with D-Bus from command shell there are some related tools, such as
`busctl` , `gdbus` , `dbus-send`, etc., which can perform some D-BUS related
operations. For example, to introspect the bus for the `systemd-resolved`
service, you could execute any of these similar commands:

``` shell
$ gdbus introspect --system --dest org.freedesktop.resolve1 --object-path /org/freedesktop/resolve1
```

or

``` shell
$ busctl introspect org.freedesktop.resolve1 /org/freedesktop/resolve1
NAME                                TYPE      SIGNATURE     RESULT/VALUE                             FLAGS
org.freedesktop.DBus.Introspectable interface -             -                                        -
.Introspect                         method    -             s                                        -
org.freedesktop.DBus.Peer           interface -             -                                        -
.GetMachineId                       method    -             s                                        -
.Ping                               method    -             -                                        -
org.freedesktop.DBus.Properties     interface -             -                                        -
.Get                                method    ss            v                                        -
.GetAll                             method    s             a{sv}                                    -
.Set                                method    ssv           -                                        -
.PropertiesChanged                  signal    sa{sv}as      -                                        -
org.freedesktop.resolve1.Manager    interface -             -                                        -
.FlushCaches                        method    -             -                                        -
.GetLink                            method    i             o                                        -
.RegisterService                    method    sssqqqaa{say} o                                        -
.ResetServerFeatures                method    -             -                                        -
.ResetStatistics                    method    -             -                                        -
.ResolveAddress                     method    iiayt         a(is)t                                   -
.ResolveHostname                    method    isit          a(iiay)st                                -
.ResolveRecord                      method    isqqt         a(iqqay)t                                -
.ResolveService                     method    isssit        a(qqqsa(iiay)s)aayssst                   -
.RevertLink                         method    i             -                                        -
.SetLinkDNS                         method    ia(iay)       -                                        -
.SetLinkDNSSEC                      method    is            -                                        -
.SetLinkDNSSECNegativeTrustAnchors  method    ias           -                                        -
.SetLinkDomains                     method    ia(sb)        -                                        -
.SetLinkLLMNR                       method    is            -                                        -
.SetLinkMulticastDNS                method    is            -                                        -

...

```

## The solution ##

Now we know what to touch in our operating system we are in a position to solve
our DNS related lack of anonymity.

We begin identifying out link number getting information through the command `systemd-resolve --status`:

``` shell
$ systemd-resolve --status
Global

  ...

Link 3 (ppp0)

  ... 
  
Link 2 (eno1)

  ...
```

In this example, the link number of the connection over which the VPN (`ppp0`) is created is `3`.

To get only the number a more customized command could be runned:

``` shell
$ systemd-resolve --status | grep ppp0 | sed 's/.*Link \([0-9]\+\).*/\1/'
3
```

Now you've got the link number, you will set `DNS Domain: ~.`. Do it calling
`SetLinkDomains()` method to set the search and routing domains to use on the
interface `ppp0` (link number `3` in the example) for DNS look-ups. It take a
network interface index plus an array of domains, each with a boolean parameter
indicating whether the specified domain shall be used as search domain (false),
or just as routing domain (true). Continuing with the example:

``` shell
$ busctl call org.freedesktop.resolve1 /org/freedesktop/resolve1 org.freedesktop.resolve1.Manager SetLinkDomains 'ia(sb)' 3 . true
```

To check the action success:

``` shell
$ busctl call org.freedesktop.resolve1 /org/freedesktop/resolve1 org.freedesktop.resolve1.Manager GetLink 'i' 3 
o "/org/freedesktop/resolve1/link/_33"
```

Run again the command `systemd-resolve --status` to observe that DNS Domain is now set to `~.`:

``` shell
$ systemd-resolve --status
Global

  ...

Link 3 (ppp0)
      Current Scopes: DNS
       LLMNR setting: yes
MulticastDNS setting: no
      DNSSEC setting: no
    DNSSEC supported: no
         DNS Servers: n.n.n.n
          DNS Domain: ~.
  
Link 2 (eno1)

  ...

```

Now, your VPN is running and your system is addressing all DNS look-ups to your
VPN. Try then <https://www.dnsleaktest.com/> and check your system is DNS leaks
free.

## Scripting ##

In order to automate the previous actions you must write a script that is
executed when the VPN starts up.

### How to run the script when the VPN connection is activated ###

Ubuntu introduced `netplan` utility to configure network on ubuntu 18.04 LTS.
[Netplan](https://netplan.io/) is defined as *the network configuration
abstraction renderer*[^5]. You simply create a YAML description of the required
network interfaces and what each should be configured to do. From this
description Netplan will generate all the necessary configuration for your
chosen renderer tool.

Netplan reads network configuration from `/etc/netplan/*.yaml` which are written
by administrators, installers, cloud image instantiations, or other OS
deployments. During early boot, Netplan generates backend specific configuration
files in `/run` to hand off control of devices to a particular networking daemon.

Netplan currently works with
[`NetworkManager`](https://en.wikipedia.org/wiki/NetworkManager) and
[`Systemd-network`](https://wiki.archlinux.org/index.php/systemd-networkd)
renderers.

In [Ubuntu 18.04, netplan renders
NetworkManager](https://blog.ubuntu.com/2017/12/01/ubuntu-bionic-netplan)
connection configuration files by default with a content like this:

``` yaml
network:
  renderer: NetworkManager
```

Ubuntu provides `systemd-networkd` which will be alternative of `NetworkManager`
too. But `systemd-networkd` service is disabled by default.

<img style="display: block;max-width: 100%;height: auto;margin-left: auto;margin-right: auto;border-radius: 8px;background-color: lightgray;" src="https://upload.wikimedia.org/wikipedia/commons/b/b1/Linux_desktop_system_daemons_and_their_graphical_front-ends.svg" alt="System daemons">

`NetworkManager` has the ability to start services when you connect to a network
and stop them when you disconnect[^10]. To activate the feature the
`NetworkManager-dispatcher.service` must be enabled and started. In Ubuntu 18.04
LTS this feature is activated by default and `NetworkManager` will run all of
the scripts in the `/etc/NetworkManager/dispatcher.d/` directory when a
connection changes (up, down, preup, predown).

Scripts must be owned by root, otherwise the dispatcher will not execute them.
For added security, you must set group ownership to root as well:

``` shell
$ chown root:root /etc/NetworkManager/dispatcher.d/10-script
```

and set the correct permissions:

``` shell
$ chmod 755 /etc/NetworkManager/dispatcher.d/10-script
```

The scripts will be run in alphabetical order at connection time, and in reverse
alphabetical order at disconnect time. To ensure what order they come up in, it
is common to use numerical characters prior to the name of the script (e.g.
10-portmap or 30-netfs (which ensures that the portmapper is up before NFS
mounts are attempted).

### The script ###

In [this link](https://github.com/systemd/systemd/issues/6076) you will find the
following script:

``` shell
#!/bin/bash

IF=$1
STATUS=$2

# contains(string, substring)
#
# Returns 0 if the specified string contains the specified substring,
# otherwise returns 1.
contains() {
    string="$1"
    substring="$2"
    if test "${string#*$substring}" != "$string"
    then
        return 0    # $substring is in $string
    else
        return 1    # $substring is not in $string
    fi
}

function notify-send() {
    #Detect the name of the display in use
    local display=":$(ls /tmp/.X11-unix/* | sed 's#/tmp/.X11-unix/X##' | head -n 1)"

    #Detect the user using such display
    local user=$(who | grep '('$display')' | awk '{print $1}')

    #Detect the id of the user
    local uid=$(id -u $user)

    sudo -u $user DISPLAY=$display DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/$uid/bus notify-send "$@"
}

case "$STATUS" in
    vpn-up)
      # If the VPN connection name does not contain 'MyVPN' return from the script
      contains "${CONNECTION_ID}" "MyVPN" || exit 0
      # Get Link number from systemd-resolve for the new interface
      LINKNUM=`systemd-resolve --status | grep $VPN_IP_IFACE | sed 's/.*Link \([0-9]\+\).*/\1/'`
      case $LINKNUM in
      ''|*[!0-9]*)
        # Bad number, notify the user... 
        notify-send "VPN DNS Warning" "Could not find Link number systemd-resolve for ${VPN_IP_IFACE}"
      ;;
      *)
        # We have a link number, lets set DNS Domain: ~.   
        busctl call org.freedesktop.resolve1 /org/freedesktop/resolve1 org.freedesktop.resolve1.Manager SetLinkDomains 'ia(sb)' $LINKNUM 1 . true
        # Check if setting was successfully set
        LOBJECT=`busctl call org.freedesktop.resolve1 /org/freedesktop/resolve1 org.freedesktop.resolve1.Manager GetLink 'i' $LINKNUM | sed 's/o.\+"\(.*\)"/\1/'`
        LDOMAINS=`busctl get-property org.freedesktop.resolve1 $LOBJECT org.freedesktop.resolve1.Link Domains`
        if (echo $LDOMAINS | grep "\".\" true"); then
          notify-send "VPN DNS set successfully" "All DNS requests are routed through ${VPN_IP_IFACE}"
        else
          notify-send -i dialog-error "VPN DNS Error" "DNS Domains . could not be set for ${VPN_IP_IFACE}"
        fi
      ;;
      esac
```

Write a file with the previous code that is called, for example, `10-vpn-up` and
place it in the `/etc/NetworkManager/dispatcher.d/` directory:

``` shell
$ sudo vim /etc/NetworkManager/dispatcher.d/10-vpn-up
$ sudo chown root:root /etc/NetworkManager/dispatcher.d/10-vpn-up
$ sudo chmod 755 /etc/NetworkManager/dispatcher.d/10-vpn-up
```

This script will be executed by `NetworkManager` every time there was a
connection change checking if the vpn connection id contains a certain substring
(your VPN name or a common substring for all your DNS leaks free VPNs - `MyVPN`
in this example). If you succeed, the DNS Domain is set for the new VPN connection and no
DNS leaks any more.

 


## Footnotes ##

[^1]: Mason, Andrew G. (2002). Cisco Secure Virtual Private Network. Cisco Press. p. 7.

[^2]: <https://www.howtogeek.com/133680/htg-explains-what-is-a-vpn/>

[^3]: <https://www.freedesktop.org/software/systemd/man/systemd-resolved.service.html>

[^4]: <https://en.wikipedia.org/wiki/Resolv.conf>

[^5]: <https://netplan.io/>

[^6]: <https://www.freedesktop.org/software/systemd/man/resolved.conf.html>

[^7]: <https://www.freedesktop.org/wiki/Software/systemd/resolved/>

[^8]: <https://hustcat.github.io/getting-started-with-dbus/>

[^9]: <https://pythonhosted.org/txdbus/dbus_overview.html>

[^10]: <https://wiki.archlinux.org/index.php/NetworkManager#Network_services_with_NetworkManager_dispatcher>
