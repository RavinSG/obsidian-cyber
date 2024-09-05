Acquiring a forensic copy of a drive or device requires a tool that can create a complete copy of the device at a bit-for-bit level. Examples of such tools are: **dd**, **FTK Imager**, and **WinHex**.

In Linux, **dd** is a command-line utility that allows you to create images for forensic or other purposes. The dd command line takes input such as an input location (`if`), an output location (`of`), and flags that describe what you want to do, such as create a complete copy despite errors.

To copy a drive mounted as `/dev/sda` to a file called `example.img`, you can execute a command like the following:

```bash
dd if=/dev/sda of=example.img conv=noerror,sync
```

Additional settings are frequently useful to get better performance, such as *setting the block size appropriate for the drive*. If you want to use dd for forensic purposes, it is worth investing additional time to learn how to adjust its performance using block size settings for the devices and interfaces that you use for your forensic workstation.

>[!info]
>If you are creating a forensic image, you will likely want to *create an MD5sum hash of the image as well*. To do that, you can use pipes, the `tee` command, and `md5sum`:
>
`dd if=/dev/sda bs=4k conv=sync,noerror | tee example.img | md5sum> example.md5` 
>
>This command will image the device at `/dev/sda` using a 4k block size, and will then run an MD5sum of the resulting image that will be saved as `example.md5`. Hashing the original drive (`/dev/sda`) and comparing the hashes will let you know if you have a valid forensic image.

**FTK Imager** is a free tool for creating forensic images. It supports *raw* (dd)-style format as well as *SMART* (ASR Data's format for their SMART forensic tool), *E01* (EnCase), and *AFF* (Advanced Forensics Format) formats commonly used for forensic tools. 

Understanding what format you need to produce for your analysis tool and whether you may want to have copies in more than one format is important when designing your forensic process.

Physical drives, logical drives, image files, and folders, as well as multi-CD/DVD volumes are all supported by FTK Imager. In most cases, *forensic capture is likely to come from a physical or logical drive*. 

![[FTK Imager.png]]

The above image shows a completed image creation from a physical drive using FTK Imager. Note the *matching and validated MD5 and SHA1 hashes* and confirmation that there were no bad blocks, which would indicate potential data loss or problems with the drive.

In addition to drive imaging tools, forensic analysts are sometimes asked to *capture live memory* on a system. Along with drive images, FTK Imager can capture live memory from a system, as shown below, the simple GUI lets you select where the file will go, the filename, whether the system pagefile for *virtual memory should be included*, and whether to save it in the AD1 native FTK file format.

![[FTK Memory Capture.png]]

Another useful forensic tool is **WinHex**, *a disk editing tool* that can also acquire disk images in raw format, as well as its own dedicated WinHex format. WinHex is useful for directly reading and modifying data from a drive, memory, RAID arrays, and other filesystems.

### [[Acquiring Network Forensic Data]]

### [[Acquiring Forensic Information from Other Sources]]