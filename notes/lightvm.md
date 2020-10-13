# My VM is Lighter (and Safer) than your Container

This paper presents LightVM, a new virtualization solution based on Xen that is optimized to offer fast boot-times regardless of the number of active VMs.

## Motivations

Containers are lightweight compared to virtual machines but offers weaker isolation than VMs. Can we have the improved isolation of VMs, with the efficiency of containers?

## Solution

The goal is to offer fast boot-times regardless of the number of active VMs.

### Unikernel and Tinyx build system

* __Observation 1__: Image size matters a lot for fast booting.
* __Observation 2__: Most containers and virtual machines run a single application.
* __Solution__: Unikernel (tiny virtual machines where a minimalistic operating system is linked directly with the target application).
* __Problem__: Making unikernel is complicated.
* __Solution__: Tinyx, an automated build system that creates minimalistic Linux VM images targeted at running a single application.

### noxs, a distributed implementation of Xen’s centralized toolstack architecture based on the XenStore

* __Observation__: XenStore interaction and device creation dominate after using unikernels. The centralized protocol is quite expensive.
* __Solution__: Redesign the Xen control plane with a lean driver called noxs (for 'no XenStore') that replaces the XenStore and allows direct communication between front-end and back-end drivers via shared memory.

## Evaluation

A performance evaluation of LightVM with comparisons to standard Xen and, where applica-
ble, Docker containers are presented using three different type of guests: Mini-OS-based unikernels, Tinyx images, and a Debian VM.

* __Boot time__: Unikernel is much faster than others while the boot time of a Tinyx guest is fairly close to Docker containers.
* __Checkpointing__: LightVM can save a VM in around 30ms and restore it in 20ms, regardless of the number of running guests, while standard Xen needs 128ms and 550ms respectively.
* __Memory footprint__: The unikernel’s memory usage is fairly close to that of Docker containers. The Tinyx curve is higher, since multiple copies of the Linux kernel are running; however, the additional memory consumption is not dramatic.
* __CPU usage__: CPU usage can also be on a par with containers with unikernels and Tinyx images.

## Comments

Based on the observation that most containers and virtual machines run a single application, the authors managed to use VMs with minimal functionality to reduce both the image size and the memory footprint of virtual machines. This is a nice catch and a good example of making trade-offs.
