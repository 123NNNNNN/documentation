---
title: Parsing Nginx Access Logs with GoAccess
description: Learn how to parse the nginx-access.log file with GoAccess to gather information on your visitors and referral traffic.
tags: [logs, nginx, goacess]
categories: [performance]
contributors: [albertcausing, sarahg]
reviewed: "2020-01-09"
---
Pantheon runs nginx web servers for optimal performance. Your site's nginx access logs record web server events and activities that can help you identify potential issues and gather information about users.

<Alert title="Note" type="info">

Requests served by the [Pantheon Global CDN](/global-cdn) will not hit the nginx webserver and will not be logged in `nginx-access.log`.

</Alert>

[GoAccess](https://goaccess.io/) is a free, open source utility that creates reports by parsing `nginx-access.log` files. Use it to quickly identify the most used browsers and operating systems, visitor IPs, or most frequent 404s — all from the command line.

## Before You Begin

Be sure that you have:

* [Terminus](/terminus)
* [GoAccess](https://goaccess.io/download)
  * **Mac OS X**: Install via [Homebrew](https://brew.sh/) (`brew install goaccess`)
  * **Windows**: Use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
  
This guide is written for the latest stable release of GoAccess as of this writing, which is version 1.3 ([release notes](https://goaccess.io/release-notes#release-1.3)).

## Edit GoAccess Configuration

To parse your `nginx-access.log` files with GoAccess, you'll need to configure GoAccess to read Pantheon's log formats.

The configuration file is located under `~/.goaccessrc` or `%sysconfdir%/goaccess.conf` where `%sysconfdir%` is either `/etc/`, `/usr/etc/` or `/usr/local/etc/` ([read more](https://goaccess.io/faq#configuration)).

Add the following lines to the `goaccess.conf` file:

```conf:title=goaccess.conf
time-format %T
date-format %d/%b/%Y
log-format %h - %^ [%d:%t %^]  "%r" %s %b "%R" "%u" %T "%^"
```

## Create a report

1. [Download your nginx log files](/logs) from Pantheon via SFTP.
1. From the directory containing your `nginx-access.log` file, run GoAccess:

  ```bash{promptUser: user}
  goaccess nginx-access.log
  ```

  You can use the arrow keys on your keyboard to scroll down to view more of the report, or hit `q` to exit.

  Alternatively, you can generate an HTML report:

  ```bash{promptUser: user}
  goaccess nginx-access.log > report.html
  ```

1. View the report in your browser by opening `report.html`. For MacOS:

  ```bash{promptUser: user}
  open report.html
  ```

  For Linux:

  ```bash{promptUser: user}
  xdg-open report.html
  ```

### Troubleshooting "goaccess.conf Not Found"
In certain MacOS [Homebrew](https://brew.sh/) installations of GoAccess, `goaccess.conf` is not found by the binary. To resolve:

1. Display the default config path:

  ```bash{promptUser: user}
  goaccess --dcf
  ```

1. Move `goaccess.conf` from `/usr/local/Cellar/goaccess/[VERSION]/conf/etc/goaccess` to `/usr/local/Cellar/goaccess/[VERSION]/conf/etc`:

  ```bash{promptUser: user}
  mv /usr/local/Cellar/goaccess/[VERSION]/conf/etc/goaccess /usr/local/Cellar/goaccess/[VERSION]/conf/etc
  ```

An [issue has been filed](https://github.com/allinurl/goaccess/issues/1640) on the GoAccess repo.

## Automate GoAccess Reports

1. Copy the general log retrieval script from [Automate Downloading Logs](/logs#automate-downloading-logs), and use this to download logs from all application containers on the desired environment.

2. Add the following to either `collect-logs.sh` or a separate file:

  ```bash
  # Unpack archived log files (optional).
  gunzip */nginx-access.log-*

  # Create a GoAccess report and open it in a browser.
  goaccess */nginx-access.log* > goaccess.html && open goaccess.html # Or xdg-open for Linux
  ```

## See Also

* [Log Files on Pantheon](/logs)
* [Bots and Indexing](/bots-and-indexing)
* [Traffic Limits and Overages](/traffic-limits)
