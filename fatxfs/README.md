
fatxfs
======
A [FUSE userspace filesystem driver](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) built using libfatx that enables you to mount a FATX filesystem on your host system and interact with it using your typical system tools. Works on Linux and macOS. Other platform support is possible, but untested.

Build via Docker
----------------
fatxfs can be easily built inside a [Docker](https://www.docker.com/) container. If you use this method, you can skip the prerequisites and build instructions below.

    $ git clone https://github.com/mborgerson/fatx
    $ docker build -t fatxfs .
    $ docker run -it --rm -v $PWD:/work -w /work --device /dev/fuse --privileged fatxfs
    # mkdir c
    # fatxfs xbox.img c
    # ls c

How to Build (Natively)
-----------------------
### Prerequisites

Note: You'll need to build and install libfatx first.

#### Ubuntu
Assuming you already have typical build tools installed, install FUSE and CMake:

    $ sudo apt-get install libfuse-dev cmake pkg-config

#### macOS
Download Xcode (available from the App Store) to get command line tools.

Assuming you have homebrew installed, install `pkgconfig` and `cmake`:

    $ brew install pkgconfig cmake macfuse

### Download Source
Clone the repository:

    $ git clone https://github.com/mborgerson/fatx && cd fatx/fatxfs

### Build
Create a build directory and run `cmake` to construct the Makefiles:

    $ mkdir build && cd build
    $ cmake ..

Finally, start the build:

    $ make

How to Use
----------
Firstly, you will need a raw disk image or block device to mount. Then, you can simply create a mountpoint and mount the "C drive" (default behavior). For example:

    $ cd fatxfs
    $ mkdir c_drive
    $ ./fatxfs /dev/nbd0 c_drive
    $ ls c_drive

You can specify the drive letter of the partition to mount:

    $ ./fatxfs /dev/nbd0 e_drive --drive=e

Or, you can specify the offset and size of the partition manually:

    $ ./fatxfs /dev/nbd0 c_drive --offset=0x8ca80000 --size=0x01f400000

To mount an XMU, use: --sector-size=4096 --offset=0 --size=XMU_SIZE_IN_BYTES

Tips
----
### Mounting a qcow Image
If your disk image is a [qcow](https://en.wikipedia.org/wiki/Qcow) image, you can mount it as a network block device before mounting a partition on the device:

    $ sudo apt-get install qemu-utils
    $ sudo modprobe nbd max_part=8
    $ sudo qemu-nbd --connect=/dev/nbd0 /path/to/your/image.qcow2
    $ sudo chmod a+rwx /dev/nbd0

Unfortunately, on macOS, there is not a way to mount a qcow image like this (AFAIK). I recommend converting the qcow image to a raw disk image.

    $ qemu-img convert /path/to/image.qcow /path/to/output.raw

### Logging
For debug purposes, you can have `fatxfs` log operations to a file given by the
`--log` option. You can control the amount of output using the `--loglevel`
option.
