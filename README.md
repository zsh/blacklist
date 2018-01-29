# UBNT edgeos-dnsmasq-blacklist dnsmasq DNS blacklisting and redirection

[![License](https://img.shields.io/badge/license-BSD-blue.svg)](https://github.com/britannic/blacklist/blob/master/LICENSE.txt)[![Alpha  Version](https://img.shields.io/badge/version-v0.0.10-green.svg)](https://github.com/britannic/blacklist)[![GoDoc](https://godoc.org/github.com/britannic/blacklist?status.svg)](https://godoc.org/github.com/britannic/blacklist)[![Build Status](https://travis-ci.org/britannic/blacklist.svg?branch=master)](https://travis-ci.org/britannic/blacklist)[![Coverage Status](https://coveralls.io/repos/github/britannic/blacklist/badge.svg?branch=master)](https://coveralls.io/github/britannic/blacklist?branch=master)[![Go Report Card](https://goreportcard.com/badge/gojp/goreportcard)](https://goreportcard.com/report/github.com/britannic/blacklist)

[community.ubnt.com](https://community.ubnt.com/t5/EdgeMAX/Self-Installer-to-configure-Ad-Server-and-Blacklist-Blocking/td-p/1337892)

NOTE: THIS IS NOT OFFICIAL UBIQUITI SOFTWARE AND THEREFORE NOT SUPPORTED OR ENDORSED BY Ubiquiti Networks®

## Copyright

* Copyright © 2018 Helm Rock Consulting

## Overview

EdgeMax dnsmasq DNS blacklisting and redirection is inspired by the users at [EdgeMAX Community](https://community.ubnt.com/t5/EdgeMAX/bd-p/EdgeMAX)

## Licenses

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
   list of conditions and the following disclaimer.
1. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
    ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
    WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
    DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
    ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
    (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
    LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
    ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
    (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
    SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

    The views and conclusions contained in the software and documentation are those
    of the authors and should not be interpreted as representing official policies,
    either expressed or implied, of the FreeBSD Project.

## Features

* Adds DNS blacklisting integration to the EdgeRouter configuration
* Generates configuration files used directly by dnsmasq to redirect dns lookups
* Integrated with the EdgeMax OS CLI
* Any FQDN in the blacklist will force dnsmasq to return the configured dns redirect IP address

## Compatibility

* edgeos-dnsmasq-blacklist has been tested on the EdgeRouter ERLite-3, ERPoe-5, ER-X and UniFi Security Gateway USG-3 routers
  * versions EdgeMAX: v1.7.0-v1.9.7+hotfix.4, UniFi: v4.4.12-v4.4.18
* integration could be adapted to work on VyOS and Vyatta derived ports, since  EdgeOS is a fork and port of Vyatta 6.3

## Installation

### EdgeRouter ERLite-3, ERPoe-5 & UniFi-Gateway-3

```bash
curl https://community.ubnt.com/ubnt/attachments/ubnt/EdgeMAX/194030/7/edgeos-dnsmasq-blacklist_0.0.10_mips.deb.tgz | tar -xvz
sudo dpkg -i edgeos-dnsmasq-blacklist_0.0.10_mips.deb
```

### EdgeRouter ER-X & ER-X-SFP

```bash
curl
https://community.ubnt.com/ubnt/attachments/ubnt/EdgeMAX/194030/8/edgeos-dnsmasq-blacklist_0.0.10_mipsel.deb.tgz | tar -xvz
sudo dpkg -i edgeos-dnsmasq-blacklist_0.0.10_mipsel.deb
```

## Upgrade

* Since dpkg cannot upgrade packages, follow the instructions under [Installation](#installation) and the previous package version will be automatically removed before the new package version is installed

## Removal

### EdgeMAX - All Platforms

```bash
sudo apt-get remove edgeos-dnsmasq-blacklist
```

## Usage

* In daily use, no additional interaction with update-dnsmasq is required. By default, cron will run update-dnsmasq at midnight each day to download the blacklist sources and update the dnsmasq configuration files in /etc/dnsmasq.d. dnsmasq will automatically be reloaded after the configuration file update is completed.
* The daily task is scheduled using these configuration commands:

```bash
set system task-scheduler task update_blacklists executable path /config/scripts/update-dnsmasq
set system task-scheduler task update_blacklists interval 1d
```

* update-dnsmasq has the following commandline switches available:

```bash
/config/scripts/update-dnsmasq.amd64 -h
    -dir string
            Override dnsmasq directory (default "/etc/dnsmasq.d")
    -f [full file path]
            [full file path] # Load a config.boot file
    -h   Display help
    -v   Verbose display
    -version
            Show version
```

### EdgeOS dnsmasq Configuration

* dnsmasq may need to be configured to ensure blacklisting works correctly
  * Here is an example using the EdgeOS configuration shell

```bash
configure
set service dns forwarding cache-size 2048
set service dns forwarding except-interface [Your WAN i/f]
set service dns forwarding name-server [Your choice of IPv4 Internet Name-Server]
set service dns forwarding name-server [Your choice of IPv4 Internet Name-Server]
set service dns forwarding name-server [Your choice of IPv6 Internet Name-Server]
set service dns forwarding name-server [Your choice of IPv6 Internet Name-Server]
set service dns forwarding options bogus-priv
set service dns forwarding options domain-needed
set service dns forwarding options domain=mydomain.local
set service dns forwarding options enable-ra
set service dns forwarding options expand-hosts
set service dns forwarding options localise-queries
set service dns forwarding options strict-order
set service dns forwarding system
set system name-server 127.0.0.1
set system name-server '::1'
commit; save; exit
```

## Releases

### Patch v0.0.9

* Added logging for download errors and warnings for empty content
* Change HTTP user agent to emulate curl, to stop web servers from offering complex content
* Removed embedded tabs in source prefixes that were interpreted by the EdgeOS configure shell as a completion request,  preventing correct prefix matches

### Patch v0.0.8

* Removes redundant references to blacklist.t and perl modules
* Replace "▶" with ":" in log messages

> blacklist





- - -
Generated by [godoc2md](http://godoc.org/github.com/davecheney/godoc2md)
