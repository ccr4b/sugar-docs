Setup a development environment
===============================

Sugar is made of several modules and depends on many other libraries.

There are several ways to set up a Sugar environment for doing Sugar
development, choose one at a time only;

* for testing or changing Sugar or a Sugar activity, install a [live
  build](#LIVE), which has all dependencies and source code included,
  but is nearly 1GB of downloads;

* for writing or changing a Sugar activity, install a [packaged Sugar
  environment](#PACKAGED), which will install dependencies
  automatically; or,

* for packaging Sugar, downstream developers create a [native Sugar
  build](#NATIVE) and install the necessary dependencies by hand,
  but Sugar is difficult to remove.

<a name="LIVE">
Sugar Live Build
----------------
</a>

Sugar Live Build is a complete bootable image containing Sugar, the
toolkits, and the demonstration activities;

* can be booted from hard drive, flash drive, and optical media,
  automatically starting Sugar without persistence,

* can be installed as a virtual machine, with persistence and password
  protection,

* contains all build dependencies, configured source trees (git clones
  in `/usr/src`), and binaries (`make install`) for Sugar modules and
  the demonstration activity set.

See
[downloads](http://people.sugarlabs.org/~quozl/sugar-live-build-20171009/)
for the ISO9660 image file.

Once installed, Sugar Live Build can be used to make changes to Sugar,
the toolkits, the demonstration activities, or to write new
activities.

* changes to Sugar or the toolkits can be done by editing files in the
  module source trees in `/usr/src`, followed by `sudo make install`
  for each changed module.

* changes to demonstration activities can be done in the activity
  source trees in `/usr/src/sugar-activities`, and are immediately
  effective; just start a new instance of the activity in Sugar.

* writing new activities can be done in the `~/Activities/` directory,
  and the new activity can be started using `sugar-activity` command
  in Terminal, or by restarting Sugar so that the new
  `activity/activity.info` file is read to regenerate the [Home
  View](https://help.sugarlabs.org/en/home_view.html).

See [sugar-live-build](https://github.com/sugarlabs/sugar-live-build)
on GitHub for configuration files to make your own Sugar Live Build
using the Debian Live Build software.

<a name="PACKAGED">
Packaged Sugar
--------------
</a>

For development of activities without making changes to Sugar desktop.

For Fedora users, see [Using Sugar on
Fedora](http://wiki.sugarlabs.org/go/Fedora).  Once Sugar is
installed, development of activities can begin.

For Debian users, see also [Using Sugar on
Debian](http://wiki.sugarlabs.org/go/Debian), or see how to install
`sucrose` below.

For Ubuntu users, see also [Using Sugar on
Ubuntu](http://wiki.sugarlabs.org/go/Ubuntu), or see how to install
`sucrose` below.

Install Debian **Stretch** or **Buster**, or Ubuntu 17.10 **Artful**.

Install the `sucrose` package;

```
sudo apt install sucrose
```

Log out, then log in with the Sugar desktop selected.

Or, install `xrdp` and `rdesktop` then log in to Sugar in a window;

```
sudo apt install xrdp rdesktop
sudo adduser guest
echo sugar | sudo tee -a /home/guest/.xsession
rdesktop -g 1200x900 -u guest -p guest 127.0.0.1
```

Once you have a local Sugar desktop, or a remote Sugar desktop,
development of activities can begin.

<a name="NATIVE">
Native Sugar
------------
</a>

For experts.

Clone each of the module repositories;

```
for module in sugar{-datastore,-artwork,-toolkit,-toolkit-gtk3,}; do
    git clone https://github.com/sugarlabs/$module.git
done
```

Install the build dependencies.  There are many, and their package
names vary by distribution.  A good list is in the Debian or Fedora
packaging files.

On Debian or Ubuntu;

```
for module in sugar{-datastore,-artwork,-toolkit,-toolkit-gtk3,}; do
    apt build-dep $module
done
```

On Fedora, use [dnf builddep](http://dnf-plugins-core.readthedocs.io/en/latest/builddep.html), like this;

```
for module in sugar{-datastore,-artwork,-toolkit,-toolkit-gtk3,}; do
    dnf builddep $module
done
```

Autogen, configure, make, and install each module;

```
for module in sugar{-datastore,-artwork,-toolkit,-toolkit-gtk3,}; do
    cd $module
    ./autogen.sh
    ./configure
    make
    sudo make install
    cd ..
done
```

Clone the Browse and Terminal activities;

```
mkdir -p ~/Activities
cd ~/Activities
git clone https://github.com/sugarlabs/browse-activity.git Browse.activity
git clone https://github.com/sugarlabs/terminal-activity.git Terminal.activity
```

Log out and log in again with the Sugar desktop selected, or use the
remote desktop feature described earlier on this page.

After making changes in a Sugar module, repeat the `sudo make install`
step, and log in again.
