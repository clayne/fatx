fatx
====

Original Xbox FATX Filesystem Library, Python bindings, FUSE driver, and GUI explorer.

* [**libfatx**](libfatx/README.md) is a C library for working with the FATX filesystem, a variant of FAT16/32 developed by Microsoft for the original Xbox console.
* [**fatxfs**](fatxfs/README.md) is a [FUSE driver](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) built using libfatx.
* [**pyfatx**](pyfatx/README.md) is a Python module providing bindings to libfatx.
* [**gfatx**](gfatx/README.md) is a graphical utility for working with FATX disk images, built around libfatx.

There is work-in-progress towards a Rust implementation:
* [**fatx**](rust/fatx/README.md) is a Rust crate for working with the FATX filesystem, like libfatx.
* [**fatx-fuse**](rust/fatx-fuse/README.md) is a Rust crate providing a FUSE driver based on fuser and fatx.

License
-------
See LICENSE.txt

Credit
------
This project was made possible in part by the research done by Andrew de Quincey, Lucien Murray-Pitts, and Michael Steil. Thank you!
