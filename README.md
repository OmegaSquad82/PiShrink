
# PiShrink #

PiShrink is a bash script that automatically shrink a pi image that will then resize to the max size of the SD card on boot. This will make putting the image back onto the SD card faster and the shrunk images will compress better. 
In addition the shrinked image can be compressed with gzip and xz to create an even smaller image. Parallel compression of the image using multiple cores is supported. 

## Usage ##

```
Usage: $0 [-adhrspvzZ] imagefile.img [newimagefile.img]

  -s         Don't expand filesystem when image is booted the first time
  -v         Be verbose
  -r         Use advanced filesystem repair option if the normal one fails
  -z         Compress image after shrinking with gzip (Default is balanced (6) compression)
  -Z         Compress image after shrinking with xz (Default is balanced (6) compression)
  -c         Change the compression level from the default, balanced (6) to fast (1 for gzip, 0 or xz)
  -C         Change the compression level from the default, balanced (6) to best (9)
  -e         Compress image in extreme mode (Only applicable to xz compression)
  -a         Compress image in parallel using multiple cores
  -p         Remove logs, apt archives, dhcp leases and ssh hostkeys
  -d         Write debug messages in a debug log file
```

If you specify the `newimagefile.img` parameter, the script will make a copy of `imagefile.img` and work off that. You will need enough space to make a full copy of the image to use that option.

* `-s` Prevents automatic filesystem expantion on the images next boot. 
* `-v` Enables more verbose output. 
* `-r` Will attempt to repair the filesystem using aditional options if the normal repair fails. 
* `-z` Will compress the image after shrinking using gzip. `.gz` extension will be added to the filename. 
* `-Z` Will compress the image after shrinking using xz. `.xz` extension will be added to the filename. 
* `-c` Will use option -1 for pigz and option -0 for xz. 
* `-C` Will use option -9. 
* `-e` Will use option -e for xz. 
* `-a` will use option -f for pigz and option -T0 for xz and compress in parallel. 
* `-d` will create a logfile `pishrink.log` which may help for problem analysis. 

Default options for compressors can be overwritten by defining PISHRINK_GZIP or PSHRINK_XZ environment variables for gzip and xz. 

## Prerequisites ##

If you are running PiShrink in VirtualBox you will likely encounter an error if you
attempt to use VirtualBox's "Shared Folder" feature. You can copy the image you wish to
shrink on to the VM from a Shared Folder, but shrinking directctly from the Shared Folder
is know to cause issues. 

If using Ubuntu, you will likely see an error about `e2fsck` being out of date and `metadata_csum`. The simplest fix for this is to use Ubuntu 16.10 and up, as it will save you a lot of hassle in the long run. 

## Installation ##

```bash
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
sudo mv pishrink.sh /usr/local/bin
```

## Example ##

```bash
[user@localhost PiShrink]$ sudo pishrink.sh pi.img
pishrink.sh v0.1.2.2
pishrink.sh: Gathering data ...
Creating new /etc/rc.local
pishrink.sh: Checking filesystem ...
rootfs: 44457/96576 Dateien (0.2% nicht zusammenhängend), 292343/386048 Blöcke
resize2fs 1.45.5 (07-Jan-2020)
pishrink.sh: Shrinking filesystem ...
resize2fs 1.45.5 (07-Jan-2020)
Die Größe des Dateisystems auf /dev/loop1 wird auf 366342 (4k) Blöcke geändert.
Das Dateisystem auf /dev/loop1 is nun 366342 (4k) Blöcke lang.

pishrink.sh: Shrinking image ...
pishrink.sh: Shrunk pi.img from 1,8G to 1,7G ratio 95,65% ...
```

## Contributing ##

If you find a bug please create an issue for it. If you would like a new feature added, you can create an issue for it but I can't promise that I will get to it. 

Pull requests for new features and bug fixes are more than welcome!
