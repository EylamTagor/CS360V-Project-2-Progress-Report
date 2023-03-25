# Progress Report: Project 2 Group 6


## QEMU Issue - Medium
#### Link to issue: https://gitlab.com/qemu-project/qemu/-/issues/413

We cloned the repository and familiarized ourselves with the codebase.

We identified the source and all of the current documentation for the four methods in question:
- load_image_targphys: hw/core/loeader.c line 119
    - load_image_targphys_as: hy/core/loader.c line 126
- get_image_size: hw/core/loader.c line 72
- event_notifier_init: util/event_notifier-posix.c
- msx_init: hw/pci/msix.c

We did some preliminary examination of these methods' possible return paths in order to understand which error values are returned and when.

Our next steps are to find all callers of each of these four methods, and filter those who do not gracefully fail or check return values using the desired boolean expression. These callers will be what we need to fix.


## Firecracker Issue - Easy
#### Link to issue: https://github.com/firecracker-microvm/firecracker/issues/3273

We cloned the repository and familiarized ourselves with the codebase.

We located the TODO instances and identified the different types (e.g. Python, Markdown, Rust) and contacted the developers asking for more specifics on the relevance of each of these types, and which ones should be implemented.

Our next steps are to identify specific TODOs to implement, install the Rust compiler and environments if appropriate, and establish a more concrete roadmap regarding the specific TODOs we are implementing.

## OSv Issue - Medium
#### Link to issue: https://github.com/cloudius-systems/osv/issues/1025

We cloned the repository and familiarized ourselves with the codebase.

We tracked the progress of this issue so far to get a better idea of what specifically needs to be implemented. Once that was established, we researched the POSIX standard for named semaphores.

Our next steps are to ensure that our C++ environments are good to go, and then to implement named semaphores following the POSIX standard. We also plan to write some tests to ensure that our implementation is working.