cubian-softrains
================

Cubian-base-r8-a10 wheezy to jessie guide, real-time audio with cubieboard 1 !

1. Install on the sd card : Cubian https://github.com/cubieplayer/cubian/wiki/Install-Cubian
```
sudo dd if=/path/to/Cubian-base-r8-a10.img of=/dev/mmcblk0 bs=4096;sync
```

2. Start the cubieboard (sd card in), login with ssh
```
ssh cubie@cubieboard_ip -p36000
```

3. Upgrade to Jessie
```
sudo nano /etc/apt/sources.list # replace wheezy with jessie, comment cubian repo
sudo apt-get update
sudo apt-get dist-upgrade # quite a long process, get a book...

sudo apt-get git
git clone https://github.com/jean-emmanuel/cubian-softrains
```

4. X Server
```

# Lightweight desktop ?

sudo apt-get install lxde

# Now you need the video driver to work under Jessie

cd ~/cubian-softrains/sunxi-mali
sudo dpkg -i *.deb
sudo cp xorg.conf /etc/X11/xorg.conf

# Yep !

```


4. Audio Tools
```
# Only jackd2 will work, don't install jackd1
sudo apt-get install jackd2 a2jmidid gladish sooperlooper guitarix libcanberra-gtk-module libcanberra-gtk0 tap-plugins calf-plugins python-webkit python-pyo python-liblo
sudo nano /etc/modules # add snd_seq in a new line to avoid ALSA sequencer error (modprobe snd_seq or reboot needed) 

# Want the Non-* Suite ? Got it packaged for jessie-armhf !

cd ~/cubian-softrains/non-suite-armhf
sudo dpkg -i *.deb
sudo ldconfig # otherwise non-mixer won't work

```

5. Misc
```
# set language & timezone

sudo dpkg-reconfigure locales 
tzselect

# clean it up

sudo apt-get autoremove
sudo apt-get clean
```


6. Install on NAND
```
sudo dd if=/path/to/cubian-softrains/cubie_nand_uboot_partition_image.bin of=/dev/nand bs=4096;sync
sudo reboot
sudo mkfs.ext4 /dev/nandb
sudo mount /dev/nandb /mnt
echo -e "/proc/*\n/dev/*\n/sys/*\n/media/*\n/mnt/*\n/run/*\n/tmp/*" | nano exclude
sudo rsync -avc --progress --exclude-from=/path/to/exclude.save / /mnt
sudo nano /mnt/boot/uEnv.txt # replace root device to /dev/nandb (otherwise the cubie will try to boot from SD)
sudo reboot
```
