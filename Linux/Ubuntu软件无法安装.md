# [software installation - Unable to install "": snap "" has "install-snap" change in progress](https://www.helplib.com/article_159776)



**Question :**

I just finished the installation of the Ubuntu 18.04, but whenever i try to install any application from Ubuntu Software the same error occurs (for example"vlc") :

unable to install"vlc": snap"vlc"has"install-snap"change in progress

I hope somebody can tell me what i've done wrong.

------

**Answer 1 :**

Try using apt from the terminal.

复制代码

```
sudo apt install packagename
```

For example, if packagename is vlc then

复制代码

```
sudo apt install vlc
```

If you prefer a gui, try synaptic.if synaptic doesn't work for some reason, go to packages.ubuntu.com, look for your package and then download it off there.when you have fully downloaded it, open a terminal and cd to where you downloaded the package.

复制代码

```
sudo dpkg -i packagename.deb
```

This will install your. deb package into your system.you might want to do this before you follow these steps.

复制代码

```
sudo apt purge vlc
```

------

**Answer 2 :**

Snap is probably still working on something in the background (or at least it thinks so).Open a terminal and run `snap changes` so see a list of ongoing changes.

复制代码

```
$ snap changes
...
123 Doing 2018-04-28T10:40:11Z - Install"foo" snap
...
```

You can abort ongoing change(s) :

复制代码

```
sudo snap abort 123
```

Then you should be able to successfully install VLC through the software center.

------

**Answer 3 :**

Open your terminal and follow these steps.

## 1. Abort the"vlc"snap process.

Inspect your snap"vlc"process by running command `snap changes`, this will show the status list of the snaps installations similar to this.

复制代码

```
ID Status Spawn Ready Summary
3 Done today at 22:29 WIB today at 22:31 WIB Auto-refresh 6 snaps
4 Done today at 22:56 WIB today at 22:58 WIB Install"gitter-desktop" snap
5 Done today at 22:59 WIB today at 22:59 WIB Disconnect gitter-desktop:home from :
6 Done today at 22:59 WIB today at 22:59 WIB Disconnect gitter-desktop:pulseaudio from :
7 Doing today at 23:21 WIB - Install"spotify" snap
8 Doing today at 23:24 WIB - Install"vlc" snap
```

## 2. Pick the ID of your VLC snap process

Pick the ID of your"vlc"snap process, for the example `8`

## 3. Abort the snap process by ID

Abort snap process by running command `snap abort 8`.this action will abort your vlc snap installation process.

## 4. Open your Software Center or running snap installation by a terminal

复制代码

```
sudo snap install vlc
```

## 5. Wait for the installation until finished.