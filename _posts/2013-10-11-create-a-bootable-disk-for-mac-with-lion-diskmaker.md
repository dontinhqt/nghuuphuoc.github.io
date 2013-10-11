---
layout: post
title:  "Create a bootable disk for Mac with Lion DiskMaker"
date:   2013-10-11 14:09:00
---

Yesterday, I upgraded my iMac to MacOS Mavericks Gold Master. Most of development tools such as PHP Storm, WebStorm are working fine.
But I realized that some of libraries/tools are not compatible with this MacOS version.
And this morning, I decided to reinstall the Mountain Lion.

As usual, we can use the built-in __Disk Utility__ to create a bootable USB or hard drive. In this article, I will show you how to do it
with another free, friendly software named [Lion DiskMaker](http://liondiskmaker.com).

* Download [Lion DiskMaker](http://liondiskmaker.com)

> At the time of writing this, Lion DiskMaker supports MacOS 10.6/7/8

* Copy the Lion DiskMaker to your _Applications_ directory.

* Run the application. Choose the MacOS version which you want to create a boot disk for.

![Choose MacOS version](/img/lion-diskmaker-choose-version.png)

* In the next step, click the _Select and Install file_ button and choose the installer file.

![Select installer file](/img/lion-diskmaker-select-file.png)

* In the next step, click the _Create a boot disk_ button.

![Create a boot disk](/img/lion-diskmaker-create-boot-disk.png)

* In the next step, choose the disk that you want to store the installer.

![Choose disk](/img/lion-diskmaker-choose-disk.png)

* Wait while Lion DiskMaker does its job.

![Creating disk progress](/img/lion-diskmaker-progress.png)

* After the process completed, click the _Quit_ button. You can click the _Open Startup Disk Preference_ button to choose the Startup disk.

![Done](/img/lion-diskmaker-done.png)

After performing the steps above, my Mountain Lion installer are created on external drive as following:

![Mountain Lion installer on external drive](/img/lion-diskmaker-mountain-lion.png)

So, how easy and convenient Lion DiskMarker does for me!

Now, the next step is that restart the Mac, drink a cup of coffee, and wait for the installation to complete.