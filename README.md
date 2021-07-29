# Moulin project files

This branch contains
[Moulin](https://moulin.readthedocs.io/en/latest/) project files.

`prod-devel-rcar.yaml` contains new RCAR-based Xen Troops development
project. Work is still in progress, but right now the following features are tested and working:

* Renesas Salvator-XS M3 with 8GB memory is supported
* GPU sharing between domains
* 3 domains are being built: Linux based Dom0, DomD and DomU
* Graphics back-end in DomD
* Networking in DomD and DomU
* Network (NFS) boot for DomD and DomU
* OP-TEE client in DomU

Features that are present but not tested:

* Renesas Salvator-XS M3 with 4GB memory
* Renesas Salvator-XS H3 with 8GB memory
* Audio back-end
* SD or eMMC boot

Features that are planned, but not present:

* Virtualized OP-TEE build
* ARM-TF that boots into EL2
* AGL support
* Multimedia (HW-assisted video decoding/encoding) support

# Building
## Requirements

1. Ubuntu 18.0+ or any other Linux distribution which is supported by Poky/OE
2. Development packages for Yocto. Refer to [Yocto
   manual](https://www.yoctoproject.org/docs/current/mega-manual/mega-manual.html#brief-build-system-packages).
3. You need `Moulin` installed in your PC. Recommended way is to
   install it for your user only: `pip3 install --user
   git+https://github.com/xen-troops/moulin`. Make sure that your
   `PATH` environment variable includes `${HOME}/.local/bin`.
4. Ninja build system: `sudo apt install ninja-build` on Ubuntu

## Building

Moulin is used to generate Ninja build file: `moulin prod-devel-rcar.yaml`. This project have provides number of additional options. You can use check them with `--help-config` command line option:

```
# moulin prod-devel-rcar.yaml --help-config
usage: moulin prod-devel-rcar.yaml
       [--MACHINE {salvator-x-m3,salvator-xs-m3-2x4g,salvator-xs-h3}]
       [--ENABLE_MM {no}] [--PREBUILT_DDK {no}]

Config file description: Xen-Troops development setup for Renesas RCAR Gen3
hardware

optional arguments:
  --MACHINE {salvator-x-m3,salvator-xs-m3-2x4g,salvator-xs-h3}
                        RCAR Gen3-based device
  --ENABLE_MM {no}      Enable Multimedia support
  --PREBUILT_DDK {no}   Use pre-built GPU drivers
```

To built for Salvator XS M3 8GB use the following command line:
`moulin prod-devel-rcar.yaml --MACHINE salvator-xs-m3-2x4g`.

Moulin will generate `build.ninja` file. After that - run `ninja` to
build the images. This will take some time and disk space, as it will
built 3 separate Yocto images.
