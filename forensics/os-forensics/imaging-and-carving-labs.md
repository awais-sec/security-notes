# Imaging and File Carving Labs

Two hands-on labs: creating a forensic image of a USB drive in Linux, then carving a deleted JPEG out of a raw `.dd` image using header/footer analysis.

## Lab 1: Creating a USB Image in Linux

**Step 1**: Format a USB drive (under 16GB).

**Step 2**: Download 4-5 pictures and copy them onto the formatted USB drive.

**Step 3**: Open the drive in Kali Linux and delete the pictures.

**Step 4**: Run `lsblk` to identify the USB drive's device path. It'll show up as something like `sdX1`, where `X` is a letter specific to the device.

**Step 5**: Unmount the drive before imaging:
```
sudo umount /dev/sdX1
```
(replace `sdX1` with the actual drive name)

**Step 6**: Create the image:
```
sudo dd if=/dev/sdX of=~/usb_image.dd bs=4M status=progress
```

**Step 7**: Verify the image integrity by comparing MD5 hashes of the image and the original device:
```
md5sum ~/usb_image.dd
sudo md5sum /dev/sdX
```
(replace `sdX` with the actual drive name). If the two MD5 values match, the image was created successfully. If not, redo the imaging.

## Lab 2: Picture Recovery from a `.dd` Image

Goal: recover a deleted JPEG from a raw disk image (`awais.dd`) by locating its header and footer manually, without a forensic suite.

### Procedure

1. Create an image with a `.dd` extension.
2. Run `grep JFIF <image name.dd>` to confirm the presence of JFIF (JPEG) files.
3. Run `grep -oba JFIF <image name.dd>` to find the byte offset of the picture(s).
4. Run `xxd <image name.dd> | grep tail` to locate the footer.
5. Run `dd if=<image name.dd> of=<output.extension> bs=512 skip=<starting sector> count=<sector count>` to extract the file.

### Step 1: Find the header offset

```
grep -oba JFIF awais.dd
```
Output:
```
16810044:JFIF
4797070982:JFIF
```
(the second match here got cut off by a `zsh: killed` interruption, so only the first offset was used)

Convert the starting byte offset into a sector number by dividing by 512:
```
16810044 / 512 = 32,832
```

### Step 2: Find the footer offset

```
xxd awais.dd | grep 'd9 ff'
```
This located the JPEG end-of-image marker at multiple points in the hex dump. The relevant one gave a hex offset of `01086500`, which converts to decimal `17327360`.

Convert that ending byte offset into a sector number:
```
17327360 / 512 = 33,842
```

### Step 3: Calculate the sector count

```
32,832 - 33,842 = -1,010
```
(the negative sign is disregarded, giving a sector count of 1,010)

### Step 4: Extract the file

```
dd if=awais.dd of=awais008.jpg bs=512 skip=32832 count=1010
```
Output:
```
1010+0 records in
1010+0 records out
517120 bytes (517 kB, 505 KiB) copied, 0.0305317 s, 16.9 MB/s
```

### Step 5: Confirm recovery

The recovered file, `awais008.jpg`, appeared in the working directory alongside the original `awais.dd` image and successfully opened as a valid image, confirming the header/footer math and extraction were correct.
