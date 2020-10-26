---
title: "Notes on Sorting Photos"
date: 2019-12-23T11:50:00
tags: ["Android", "Formats", "Guides", "Linux", "Media", "Minimalism", "Snippets", "Software"]
---

My smart phone is an android device, it connects to my computer via MTP over USB and I store my photographs on cloud storage. Subsequently I want these pictures to be sorted, and I don't want any duplicates.

This entire process is structured around using ArchLinux. Below is how I complete this:

# Mount your phone's storage
I use `simple-mtpfs` to access my phone, this is installed from the AUR using `yay`:
```
yay -S simple-mtpfs
simple-mtpfs -l
mkdir phone
simple-mtpfs --device 1 phone
ls phone
```
This should now print out the file and folder structure of your phone.

# Install Phockup
Now we're going to install `phockup` to sort the files into a coherent folder structure:
```
sudo pacman -S perl-image-exiftool python3
curl -L https://github.com/ivandokov/phockup/archive/latest.tar.gz -o phockup.tar.gz
tar -zxf phockup.tar.gz
sudo mv phockup-* /opt/phockup
sudo ln -s /opt/phockup/phockup.py /usr/local/bin/phockup
```

# Mount your storage
Now we're going to mount our cloud storage destination, using the ever useful `rclone`:
```
mkdir pictures
rclone mount drive-pictures: pictures --daemon
```

# Sort your images
Now we can invoke `phockup` to sort and move the source files to the destination folder:
```
phockup phone/DCIM/Camera/ pictures/personal/photos/ -m
mv phone/Pictures/Screenshots/* pictures/personal/screenshots/ -v
rm -rf phone/DCIM/.thumbnails
find phone -maxdepth 2 -type d -not -path "*/\.*" -empty -delete
```
I also move my screenshots, as I like these being backed up but I don't care about how they're sorted.
I also delete unused thumbnails and empty folders, just to keep things tidy.

# Dedupe your images
Lastly, I deduplicate the images using `jdupes`. This will prompt you for every match it finds which copy you want:
```
yay -S jdupes
jdupes pictures/personal -rd
```

And you're done.

* **Edit 2020-10-26:** Improved empty directory deletion section
