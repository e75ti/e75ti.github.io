---
title: "Accessing WhatsApp and Viber raw databses and backing up"
summary: "If you've ever needed to fetch your WhatsApp or Viber database, this is how you can do it directly from the phone or from the hidden Google Drive, this is how you can do it! There's even an automated way!"
date: 2025-01-24
weight: 1
draft: false
---
### Requirements:
- adb
- rooted phone / AnBox virtual machine

*Read all of this so you understand what's being done, don't copy paste commands without fully reading as you don't need all of them.*

```
~$ adb devices
~$ adb root (and run adb remount after root if it runs successfully)
```
If you have issues with root, do:
```
~$ adb shell
~$ su
```
Then you can pull data with [this trick:](https://android.stackexchange.com/questions/85564/need-one-line-adb-shell-su-push-pull-to-access-data-from-windows-batch-file)
Well, you have to do a few commands, as I don't believe it will work in one.

You need to do:

1. `adb shell`
2. `su`
3. `cd /data/path/of/file`
4. `cp /data/path/of/file/copyme /data/local/tmp`
5. `chown -R shell.shell /data/local/tmp/copyme`
6. `exit`
7. `exit`
8. `adb pull /data/local/tmp/copyme /destination/copyme`

*Do not forget to remove `/data/local/tmp/copyme` after pull*

*shell.shell* is *owner.group*. When you copy a file using SU, the owner and group will be *root.root*.

The adb pull command runs as shell.shell and will not be able to pull the file from the temporary location. Changing it before doing pull will allow you permission.

Adb pull your /sdcard and /data/data/com.viber.voip (com.viber.voip either directly if you can get adb root or if you can't do the trick above).

Good job, we have finished the backing up portion. Now onto the restoring portion.

Log into viber on your new phone - device and just repeat everything but adb push data back in.

## Todo later, will add the VM portion:

- So far the idea is to have an Anbox (if possible) or Android in VM to restore from gdrive to, then pull data out of it (selfhosted of course and open source so that the community can audit code since this program has a potential to be privacy intensive you know)
