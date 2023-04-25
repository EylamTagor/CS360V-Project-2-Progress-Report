# Project Report: Group 6


## QEMU Issue - Medium
#### Link to issue: https://gitlab.com/qemu-project/qemu/-/issues/413
#### Link to forked repository: https://gitlab.com/eylamtagor1/qemu

QEMU is an open-source machine emulator and virtualizer. It is capable of emulating various architectures, including x86, ARM, MIPS, and PowerPC, and can run operating systems and applications designed for those architectures.

Issue description:
This issue revolved around solidifying checks of the return values of four methods (load_image_targphys, get_image_size, event_notifier_init, and msix_init) in order to fail gracefully upon encountering an error.

How we solved it:
We looked through every single call to these four methods and we noted those that seemed to be improperly checking for errors. After comprising a list of those calls, we made the necessary fixed to each call one by one in order to ensure it fails gracefully. Here is a comprehensive list of every method call and the fixes we implemented:

get_image_size:
- hw/i386/x86.c:1145 -> Call seems more complex than within our scope, error checking would not be relevant and will likely cause more harm than good.
- hw/mips/fuloong2e.c:121 -> Appears to be checking return == -1. Fixed to check for all error values.
- hw/mips/loongson3_virt.c:353 -> Appears to be checking return == -1. Fixed to check for all error values.
- hw/mips/malta.c:913 -> Appears to be checking return == -1. Fixed to check for all error values.
- hw/mips/mipssim.c:89 -> Appears to be checking return == -1. Fixed to check for all error values.
- hw/pci-host/raven.c:350 -> Call seems more complex than within our scope, error checking would not be relevant and will likely cause more harm than good.
- hw/smbios/smbios.c:1255 -> Checks return == -1 and something else. Fixed the first check while retaining the other checks.
- hw/sparc/leon3.c:296 -> Guarded with a call to qemu_find_file, but doesn’t actually check for an error from get_image_size. We found that this is sufficient since any failure is automatically handled.

msix_init:
- hw/net/igbvf.c:264 -> Checking if(ret). Fixed to check for all error values.
- hw/net/vmxnet3.c:2124 -> This one is technically fine, but goes against QEMU [style guide](https://www.qemu.org/docs/master/devel/style.html#:~:text=Rationale%3A%20Yoda%20conditions%20(as%20in%20%E2%80%98if%20(1%20%3D%3D%20a)%E2%80%99)%20are%20awkward%20to%20read.%20Besides%2C%20good%20compilers%20already%20warn%20users%20when%20%E2%80%98%3D%3D%E2%80%99%20is%20mis%2Dtyped%20as%20%E2%80%98%3D%E2%80%99%2C%20even%20when%20the%20constant%20is%20on%20the%20right.). Fixed to match QEMU style.
- hw/pci/msix.c:416 -> Checking if(ret). May be correct, but could also be against QEMU style guide. Fixed to match QEMU style.
- hw/scsi/megasas.c:2396 -> No check on return value at all; this one was complicated. We added a check for the return value and for an enum called ON_OFF_AUTO following a relevant comment's instructions.
- hw/usb/hcd-xhci-pci.c:159 -> No check on return value at all; this one was complicated. We added a check for the return value and for an enum called ON_OFF_AUTO following a relevant comment's instructions.

event_notifier_init:
- block/nvme.c:751 -> Checking if(ret). Fixed to check for all error values.
- hw/hyperv/hyperv.c:410 -> Checking if(ret). Fixed to check for all error values.
- hw/hyperv/hyperv.c:424 -> Checking if(ret). Fixed to check for all error values.
- hw/hyperv/vmbus.c:1426 -> Checking if(ret). Fixed to check for all error values.
- hw/hyperv/vmbus.c:2421 -> Checking ret != 0. Fixed to check for all error values.
- hw/remote/proxy.c:57,58 -> No checking return. Fixed to check for all error values.
- hw/vfio/ccw.c:426 -> Checking if(ret). Fixed to check for all error values.
- hw/vfio/pci-quirks.c:361
- hw/vfio/pci.c:135,288,433,481,684,2743
- hw/vfio/platform.c:75,86
- hw/virtio/vhost-vdpa.c:863,869

load_image_targphys:

This function calls get_image_size, but also calls another function which seems to have more complicated reasons for errors.
- hw/alpha/dp264.c:194
Does not check return value for error
Add check and handled like the above check, should suffice because the result is necessary to continue
- hw/hppa/machine.c:382
No check
Add check and handled like the above check, should suffice because the result is necessary to continue
- hw/m68k/q800.c:672
No check
- hw/m68k/q800.c:699
Has a proper check on 707 upon further inspection
- hw/m68k/virt.c:283
No check
Add check and handled like the above check, should suffice because the result is necessary to continue
- hw/mips/boston.c:780
Checks result == -1
Changed to < 0
- hw/mips/fuloong2e.c:129
Not sure why I put this one down, it definitely checks the return normally
- hw/mips/loongson3_virt.c:364
Not sure why I put this one down, it definitely checks the return normally
- hw/mips/malta.c:928
Not sure why I put this one down, it definitely checks the return normally
- hw/mips/mipssim.c:97
Not sure why I put this one down, it definitely checks the return normally
- hw/riscv/boot.c:201
Checks return == -1
Changed to < 0
- hw/s390x/ipl.c:165
Checks return == -1
Changed to < 0
- hw/s390x/ipl.c:243
Checks return == -1
Changed to < 0

We decided to leave the following calls to load_image_targphys alone: 
hw/microblaze/boot.c:175
No check, but this looks complicated
hw/nios2/boot.c:191
No check, but this looks complicated
hw/ppc/virtex_ml507.c:274
No check, but this looks complicated
hw/m68k/mcf5208.c:313
Weird check for < 8
hw/m68k/next-cube.c:1004
Weird check for < 8

Challenges:


## Firecracker Issue - Easy
#### Link to issue: https://github.com/firecracker-microvm/firecracker/issues/3273
#### Link to forked repository: https://github.com/StonewallJohnson/firecracker

Firecracker is an open-source project that provides lightweight virtualization technology for running containers and functions in a secure and isolated manner. Developed by Amazon Web Services (AWS), Firecracker uses a microVM-based architecture to provide a secure and efficient environment for running isolated workloads. The repository includes the source code for the Firecracker hypervisor, as well as tools for building and managing Firecracker-based environments.

## OSv Issue - Medium
#### Link to issue: https://github.com/cloudius-systems/osv/issues/1025
#### Link to forked repository: https://github.com/StonewallJohnson/osv

OSv is an open-source operating system designed for cloud computing. It is built from the ground up to run on virtualized environments and offers a lightweight, efficient, and high-performance alternative to traditional operating systems. OSv's design is based on the principle of running a single application per virtual machine, enabling better resource utilization and improved security. 

