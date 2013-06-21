---
layout: post
title: "Raspberry Pi Print Server Tutorial: Part 1"
date: 2013-06-21 00:10
comments: true
categories:
---

This is part 1 of my guide to using a Raspberry Pi as a print server.

The instructions here have been tested on a Raspberry Pi Model B, with a fresh Arch Linux ARM (2013-06-06) install.

## Sources
* [Original Wiki page (now defunct and outdated)](http://web.archive.org/web/20121011030651/http://archlinuxarm.org/support/guides/applications/cups-apple-airprint)

* [CUPS setup guide](http://chschneider.eu/linux/server/cups.shtml)

## Prerequisites

This tutorial assumes you have Arch Linux installed on your Pi, and have SSH access to it.

Naturally, you'll also need a CUPS compatible printer.

## Install Packages

We're going to need the following packages:

* `cups` - The main CUPS package
* `cups-pdf` - Adds print-to-PDF functionality to CUPS
* `gutenprint` - Myriad printer drivers. [Check compatibility here](http://gimp-print.sourceforge.net/p_Supported_Printers.php)
* `avahi` - Network device discovery, required for AirPrint

Run this command, then go have a sandwich as it'll take a while.

    pacman -Sy cups cups-pdf gutenprint avahi

## Add CUPS Daemon

Arch now uses systemctl (rather than rc.conf) to manage processes, so run this to add CUPS to the daemon list.

    systemctl enable cups

## Configure Your CUPS Install

Open `/etc/cups/cupsd.conf` in your favourite terminal-based editor. There are a lot of options in there, but we only need to touch a couple for now.

Replace `Listen localhost:631` with `Listen 0.0.0.0:631`. This will make the CUPS web service available to external connections, rather than just the local machine.

Now we can manage permissions. We want to allow LAN access to the printers (`/`) and administration panel (`/admin`)

    # Restrict access to the server...
    <Location />
      Order allow,deny
      Allow @LOCAL
    </Location>

    # Restrict access to the admin pages...
    <Location /admin>
      Order allow,deny
      Allow @LOCAL
    </Location>

## Trial Run

Start CUPS:

    systemctl start cups

Visit `http://pi-hostname:631` and you should see the CUPS homepage. Yay!

## Next Time

In part 2, we'll go into the details of getting a printer connected and exposing it over the network. Finally, part 3 will cover getting AirPrint working so that you can print from iOS devices.