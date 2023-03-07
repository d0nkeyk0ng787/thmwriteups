# Mounting Forensic Image Files

### Mounting DD Image Files

Before mounting the image files, it is imperative to ensure you have ```afflib-tools``` installed. This package provides access to the Advanced Forensic Format (AFF) Toolkit which is required for the conversion of the disk image to a raw format. This package can be installed as follows:
```bash
sudo apt install afflib-tools
```

From here make a directory to act as the mount point for the images in the **/mnt** directory:
```bash
sudo mkdir /mnt/imagemountpoint
```

In the example below, the image files used are named **image_1.000**, **image_2.001** and **image_3.002** and they are stored in the folder **/Desktop/Images**.

To get a mountable **raw** file, it is necessary to run the command **affuse** against **image_1.000** like so:
```bash
sudo affuse ~/Desktop/Images/image_1.000 /mnt/imagefuse
```

To verify this image and display the partition information, simply run the following command:
```bash
mmls /mnt/imagefuse/image_1.000.raw
```

This will produce something similar to the following output:
```bash
GUID Partition Table (EFI)
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Safety Table
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  Meta      0000000001   0000000001   0000000001   GPT Header
003:  Meta      0000000002   0000000033   0000000032   Partition Table
004:  000       0000002048   0000104447   0000102400   Linux filesystem
005:  001       0000104448   0000309247   0000204800   Linux filesystem
006:  -------   0000309248   0000571391   0000262144   Unallocated
007:  002       0000571392   0008388574   0007817183   Linux filesystem
008:  -------   0008388575   0008388607   0000000033   Unallocated
```

What is requried from this output is the **Units**, which in this case are **512** bytes, and the starting sector for the 3 image files. Which are **2048** for image_1.000, **104448** for image_2.001, and **571392** for image_3.002. 

To be able to mount these files, it is necessary to make a directory in **/media** to work from for each of the files. This can be done like so:
```bash
# The is necessary to ensure the images directory is also created
sudo mkdir -p /media/images/ext4_fs0
sudo mkdir -p /media/images/ext4_fs1
sudo mkdir -p /media/images/ext4_fs2
```

This next step is dependent on the account you use. If you use the root account you shouldn't have to do this, however it won't cause any problems if you do. Essentially, this next command will give user and group ownership of the directory **/media/images** to the account you are using. The command is:
```bash
# Change kali:kali to the name of the account you are using
sudo chown -R kali:kali /media/images
```

Next, it is necessary to setup the loop devices. These loop devices allow for the mapping of the loop devices data blocks to a regular file systems data blocks. Essentially, allowing for that file system to be mounted as any physical device would be. This requries those values saved earlier from the command **mmls**. To setup these loop devices for each image file do the following:
```bash
# Ensure you replace the starting sectors and byte units with whatever values you got from the command mmls
sudo losetup -f -o $((2048*512)) /mnt/imagemountpoint/image_1.000.raw
sudo losetup -f -o $((104448*512)) /mnt/imagemountpoint/image_1.000.raw
sudo losetup -f -o $((571392*512)) /mnt/imagemountpoint/image_1.000.raw
```

You can verify the status of all loop devices using the following command:
```bash
losetup -a
```

Lastly, it is necessary to use the mount command to mount the file system on a loop device which will allow for that file system to be browsed through as you would any other file system. To do this, do the following:
```bash
# Obviously, you would change the directory names to match yours.
sudo mount /dev/loop0 /media/images/ext4_fs0
sudo mount /dev/loop1 /media/images/ext4_fs1
sudo mount /dev/loop2 /media/images/ext4_fs2
```

From there you can cd into the mounted file system and navigate around it as you would any other file system. When navigating the mounted image and running commands against it, it's important to ensure you are running it against the mounted file system and not your own file system. For example, the following block of commands will run cat against the **passwd** file on your own system, not the mounted file system:
```bash
cd /media/images/ext4_fs2
cat /etc/passwd
```

To ensure your commands are run against the mounted file system, simply add a **.** before your commands. Here is the same code block, with the fix to ensure the **passwd** file of the mounted file system is the one that is displayed:
```bash
cd /media/images/ext4_fs2
cat ./etc/passwd
```

### Mounting EWF Images

```bash
# Install libewf2
sudo apt install libewf2
# Install ewf-tools
sudo apt install ewf-tools
# Mount ewf container
sudo ewfmount <image-name>.E01 /mnt/ewf
# Mount the image
sudo mount /mnt/ewf/ewf1 /mnt/e01/ -o ro,loop
# To unmount
```
