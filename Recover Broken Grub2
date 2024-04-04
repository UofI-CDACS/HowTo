# How to recover from a broken Grub2

I was recently confronted with an inability to boot my computer due to a corrupt grub2 configuration. The reasons for the corruption are unclear. However, a couple of observations were made. First an update to Ubuntu was installed very shortly before the inability to boot was discovered. Second, prior to that a USB thumb drive that had a different Ubuntu OS installed that was selectable at boot time had been removed to use for other purposes. So we have two potentially confounding issues. An update and removable of one of the boot options that the grub configuration was supporting. In any case the exact cause is imprecise.

Regardless, it is not unreasonable to expect that this could happen. Here I attempt to provide an overview of the things I tried and ultimately succeeded in recovering my ability to boot.

## Have a backup

Going into this I wasn't highly concerned because I have a reasonable backup of my system. However, recovering a system from a backup is a time intensive process so isn't plan A.

I have two backup strategies. First all of my code is placed into Git repositories that are hosted offsite, some in GitHub others on a personal AWS server.
Second, I use the built-in Ubuntu backup utility to backup to a local network server.

Enough said, there are many good backup options. I have not always had good backups.

## The problem

The boot issue exhibited when I tried to boot to my computer to access lecture notes for the class I was teaching. This occurred approximately 15 minutes before class started. Fortunately, I had uploaded a PDF of my notes to Canvas so was able to access them using the classroom computer. The lecture was completed with a few rough edges so it wasn't a complete disaster.

When booting I was confronted with the Grub prompt.

```bash
Grub> 
```

Using my phone I attempted to see what I could do to recover. I found a few sites such as

<https://unix.stackexchange.com/questions/329926/grub-starts-in-command-line-after-reboot>

I also tried booting to another partition using the F9 key to gain access to the boot menu on my HP laptop. The key to press varies depending on vendor.

My system had a partition on it for the installer for another version of Ubuntu. When I booted to it I was able to see my files on the partition that I normally boot to so I was at this point somewhat reassured that I hadn't lost everything. I was also able to boot to the Windows partition. So the disk was fairly clearly not completely broken.

Using the aforementioned link I was unable to get things to boot. It may have been possible, but that was a path that ulitmately didn't result in success.

## Using Boot-Repair

I fairly quickly stumbled across boot-repair which is a utility that advertises the ability to fix simple boot issues.

<https://help.ubuntu.com/community/Boot-Repair>

I tried a couple of things here. First, I installed the boot-repair image on a USB disk (coincidentially the same as the aforementioned disk that I had previously removed).

I was able to boot to that and attempt to run the boot-repair. **It didn't run**. Eventually I abanded that Idea and installed the Ubuntu 22.04 version of the Ubuntu installer on the same USb stick, booted to it, installed boot-repair and got it to run. It generated a quite detailed report, which basically told me that boot-repair couldn't help me.

Along the way I had run across some other information that suggested other methods.

My choices at this point were more or less, try one of those other methods, or start the laborious process of reinstalling my system.

## Reinstall grub

Ulitmately, this is what worked. However, none of the notes I found seemed to work perfectly.

One of the complications is the UEFI boot system so many of the posts that I ran across didn't perfectly help. EFI is a newer "more secure" boot method. But being somewhat new is less well under

After searches for repair broken efi grub2. I ran across this article.

<https://techbit.ca/2018/09/repair-grub2-efi-boot/>

This isn't perfect, but explains things well enough to get most of the way to fixing things.

In broad strokes the following steps are involved.

* Use Gparted to find and verify that your system partition is OK.
* Find the devce something like sdX where X is a, b, c etc. In my case it is /dev/nvme0n1. With patitions adding a number so /dev/nvme0n1p1, /dev/nvme0n1p2, etc.
* Identify the partition that your EFI information is stored in. In my case it was /dev/nvme0n1p1, more typically, it might be /dev/sda1, or /dev/sdb1, etc.
* Identify the partition that your OS is on. In my case it was /dev/nvme0n1p5. I have a somewhat messy set of partitions.
* Mount the bits that grub-install requires.
* Reinstall grub.
* Reboot
* Tweak and update grub as required.
  
The steps in the referenced tutorial didn't work perfectly for me.

Enter, another reference:
<https://askubuntu.com/questions/831216/how-can-i-reinstall-grub-to-the-efi-partition>

From this I gleaned that I needed the following as part of the mount step:

```bash
sudo mount -t efivarfs none /sys/firmware/efi/efivars
```

So overall my steps are more or less as follows

### 1 Boot to an Ubuntu Install disk

I used an install image of the same version of the OS as I'm running. Ubuntut 24.04.4 LTS. Is may be possible to use a different version.

### 2 Find my partitions

For me the device was `/dev/nvme0n1` with the EFI partition `/dev/nvme0n1p1` and the system parition `/dev/nvme0n1p5`.  This was determined using gparted, mounting partitions and looking at them, etc.

### 3 Mount things that grub-install needs

First, the system partition that you need to boot to. For me:

```bash
sudo mount /dev/nvme0n1p5 /mnt
```

Second, verify that the EFI partition is correcct. For me:

```bash
sudo mount /dev/nvme0n1p1 /mnt/mnt/boot/efi
ls /mnt/boot/efi
```

Now we set things up to make things look right for grub-install. The -B option is a bit like an alias in that it makes mount appear to show up in another place. So in the following `/dev` etc. are basically looking like what they would if you booted into the OS normally.

For me it was:

```bash
sudo mount -B /dev /mnt/dev
sudo mount -B /dev/pts /mnt/dev/pts
sudo mount -B /proc /mnt/proc
sudo mount -B /sys /mnt/sys
sudo mount -t efivarfs none /sys/firmware/efi/efivars
```

### 4 Reinstall Grub

```bash
sudo chroot /mnt
grub-install /dev/sda
grub-install --recheck /dev/sda
update-grub
exit
```

### 5 Reboot

At this point you should be able to reboot your system. Make sure you remove the install USB drive before rebooting. If grub isn't working at this point, keep trying. It took me several iterations before I was able to boot.

In my case I had things set up to offer to boot to Windows before things went bad. Setting this back up is another issue which you can read about here:

<https://www.omgubuntu.co.uk/2021/12/grub-doesnt-detect-windows-linux-distros-fix>
