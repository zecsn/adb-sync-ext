adb-sync-ext
========

adb-sync-ext is an extended version of [adb-sync](https://github.com/google/adb-sync)
tool to synchronize files between a PC and an Android device using the ADB
(Android Debug Bridge).

Setup
=====

Android Side
------------

First you need to enable USB debugging mode. This allows authorized computers
(on Android before 4.4.3 all computers) to perform possibly dangerous
operations on your device. If you do not accept this risk, do not proceed and
try using [go-mtpfs](https://github.com/hanwen/go-mtpfs) instead!

On your Android device:

* Go to the Settings app.
* If there is no "Developer Options" menu:
  * Select "About".
  * Tap "Build Number" seven times.
  * Go back.
* Go to "Developer Options".
* Enable "USB Debugging".

PC Side
-------

* Install the [Android SDK](http://developer.android.com/sdk/index.html) (the
  stand-alone Android SDK "for an existing IDE" is sufficient). Alternatively,
  some Linux distributions come with a package named like "android-tools-adb"
  that contains the required tool.
* Make sure "adb" is in your PATH. If you use a package from your Linux
  distribution, this should already be the case; if you used the SDK, you
  probably will have to add an entry to PATH in your ~/.profile file, log out
  and log back in.
* `git clone https://github.com/zecsn/adb-sync-ext`
* `cd adb-sync-ext`
* Copy or symlink the adb-sync-ext script somewhere in your PATH. For example:
  `cp adb-sync-ext /usr/local/bin/`

Usage
=====

To get a full help, type:

```
adb-sync-ext --help
```

To synchronize your music files from ~/Music to your device, type one of:

```
adb-sync-ext ~/Music /sdcard
adb-sync-ext ~/Music/ /sdcard/Music
```

To synchronize your music files from ~/Music to your device, deleting files you
removed from your PC, type one of:

```
adb-sync-ext --delete ~/Music /sdcard
adb-sync-ext --delete ~/Music/ /sdcard/Music
```

To copy all downloads from your device to your PC, type:

```
adb-sync-ext --reverse /sdcard/Download/ ~/Downloads
```

To exclude certain files or directories use the --exclude flag (or shorthand -x)
, glob patterns are allowed, and multiple with comma separation:

```
adb-sync-ext --exclude *.jpg ~/Music /sdcard
adb-sync-ext --exclude SomeFolder,*.jpg ~/Music /sdcard
```

ADB Channel
===========

This package also contains a separate tool called adb-channel, which is a
convenience wrapper to connect a networking socket on the Android device to
file descriptors on the PC side. It can even launch and shut down the given
application automatically!

It is best used as a `ProxyCommand` for SSH (install
[SSHelper](https://play.google.com/store/apps/details?id=com.arachnoid.sshelper)
first) using a configuration like:

```
Host sshelper
Port 2222
ProxyCommand adb-channel tcp:%p com.arachnoid.sshelper/.SSHelperActivity 1
```

After adding this to `~/.ssh/config`, run `ssh-copy-id sshelper`.

Congratulations! You can now use `rsync`, `sshfs` etc. to the host name
`sshelper`.

Contributing
============

Patches to this project are very welcome.

Related Projects
================

Before getting used to this, please review this list of projects that are
somehow related to adb-sync-ext and may fulfill your needs better:

* [adb-sync](https://github.com/google/adb-sync) is the original tool to
  synchronize files between a PC and an Android device using the ADB (Android
  Debug Bridge).
* [rsync](http://rsync.samba.org/) is a file synchronization tool for local
  (including FUSE) file systems or SSH connections. This can be used even with
  Android devices if rooted or using an app like
  [SSHelper](https://play.google.com/store/apps/details?id=com.arachnoid.sshelper).
* [adbfs](http://collectskin.com/adbfs/) is a FUSE file system that uses adb to
  communicate to the device. Requires a rooted device, though.
* [adbfs-rootless](https://github.com/spion/adbfs-rootless) is a fork of adbfs
  that requires no root on the device. Does not play very well with rsync.
* [go-mtpfs](https://github.com/hanwen/go-mtpfs) is a FUSE file system to
  connect to Android devices via MTP. Due to MTP's restrictions, only a certain
  set of file extensions is supported. To store unsupported files, just add
  .txt! Requires no USB debugging mode.

Disclaimer
==========

This is not an official Google product.
