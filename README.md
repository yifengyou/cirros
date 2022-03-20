# cirros用例

![20220320_175532_52](image/20220320_175532_52.png)

## 相关站点

* <http://download.cirros-cloud.net/>
* <https://launchpad.net/cirros>
* <https://github.com/cirros-dev/cirros>

## 镜像获取

* <https://download.cirros-cloud.net/0.5.2/cirros-0.5.2-x86_64-disk.img>


## 启动脚本

```
#!/bin/bash

qemu-system-x86_64 \
	-m 128 \
	-smp 2 \
	--enable-kvm \
	-boot d \
	-hda cirros-0.5.2-x86_64-disk.img \
	-nographic

```


## 快照/还原

```
# 创建还原点/快照
qemu-img snapshot -c 'original' cirros-0.5.2-x86_64-disk.img

# 罗列还原点/快照
qemu-img snapshot -l cirros-0.5.2-x86_64-disk.img

# 还原
qemu-img snapshot -a 1 cirros-0.5.2-x86_64-disk.img
```


## 优化脚本

```
sudo su
cat >> /etc/profile << EOF
PS1='\[\e[32;1m\][\[\e[31;1m\]\u\[\e[33;1m\]@\[\e[35;1m\]\h\[\e[36;1m\] \w\[\e[32;1m\]]\[\e[37;1m\]\$\[\e[0m\] '
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* -a --color=auto'
alias ll='ls -l -a --color=auto'
alias ls='ls -a --color=auto'
alias cp='cp -i'
alias mv='mv -i'
alias rm='rm -i'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'
alias push='git push'
EOF
uuid-gen > /etc/machine-id
mkdir /etc/rc3.d/.back
mv /etc/rc3.d/*cirros* /etc/rc3.d/.back
mv /etc/rc3.d/S50-dropbear /etc/rc3.d/.back
mv /etc/rc3.d/S40-network /etc/rc3.d/.back
```

其他命令

```
# 启用网络
ifup eth0

# 启用SSHD
dropbear -R

# 关机且断电
poweroff -f

# 重启
reboot -f
```



## 启动日志

```
[root@rockylinux-ebpf ~/cirros]# ./startup-qemu.sh


SeaBIOS (version 1.13.0-2.module+el8.4.0+534+4680a14e)


iPXE (http://ipxe.org) 00:03.0 CA00 PCI2.10 PnP PMM+3FF90A40+3FED0A40 CA00



Booting from DVD/CD...
Boot failed: Could not read from CDROM (code 0003)
Booting from Floppy...
Boot failed: could not read the boot disk

Booting from Hard Disk...
GRUB Loading stage2..
[    0.000000] Linux version 5.3.0-26-generic (buildd@lgw01-amd64-039) (gcc version 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)) #28~18.04.1-Ubuntu SMP Wed Dec 18 16:40:14 UTC 2)
[    0.000000] Command line: LABEL=cirros-rootfs ro console=tty1 console=ttyS0
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Hygon HygonGenuine
[    0.000000]   Centaur CentaurHauls
[    0.000000]   zhaoxin   Shanghai  
[    0.000000] x86/fpu: x87 FPU will use FXSAVE
[    0.000000] BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009fbff] usable
[    0.000000] BIOS-e820: [mem 0x000000000009fc00-0x000000000009ffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000000f0000-0x00000000000fffff] reserved
[    0.000000] BIOS-e820: [mem 0x0000000000100000-0x000000003ffdffff] usable
[    0.000000] BIOS-e820: [mem 0x000000003ffe0000-0x000000003fffffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000feffc000-0x00000000feffffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fffc0000-0x00000000ffffffff] reserved
[    0.000000] NX (Execute Disable) protection: active
[    0.000000] SMBIOS 2.8 present.
[    0.000000] DMI: Red Hat KVM, BIOS 1.13.0-2.module+el8.4.0+534+4680a14e 04/01/2014
[    0.000000] Hypervisor detected: KVM
[    0.000000] kvm-clock: Using msrs 4b564d01 and 4b564d00
[    0.000000] kvm-clock: cpu 0, msr 13c01001, primary cpu clock
[    0.000000] kvm-clock: using sched offset of 12586991805 cycles
[    0.000008] clocksource: kvm-clock: mask: 0xffffffffffffffff max_cycles: 0x1cd42e4dffb, max_idle_ns: 881590591483 ns
[    0.000011] tsc: Detected 2399.996 MHz processor
[    0.002669] last_pfn = 0x3ffe0 max_arch_pfn = 0x400000000
[    0.002982] x86/PAT: PAT not supported by CPU.
[    0.003019] x86/PAT: Configuration [0-7]: WB  WT  UC- UC  WB  WT  UC- UC  
[    0.011697] found SMP MP-table at [mem 0x000f5c00-0x000f5c0f]
[    0.012196] check: Scanning 1 areas for low memory corruption
[    0.012496] RAMDISK: [mem 0x379b1000-0x37feffff]
[    0.012599] ACPI: Early table checksum verification disabled
[    0.012605] ACPI: RSDP 0x00000000000F5980 000014 (v00 BOCHS )
[    0.012831] ACPI: RSDT 0x000000003FFE1577 00002C (v01 BOCHS  BXPCRSDT 00000001 BXPC 00000001)
[    0.012840] ACPI: FACP 0x000000003FFE1483 000074 (v01 BOCHS  BXPCFACP 00000001 BXPC 00000001)
[    0.012846] ACPI: DSDT 0x000000003FFE0040 001443 (v01 BOCHS  BXPCDSDT 00000001 BXPC 00000001)
[    0.012850] ACPI: FACS 0x000000003FFE0000 000040
[    0.012853] ACPI: APIC 0x000000003FFE14F7 000080 (v01 BOCHS  BXPCAPIC 00000001 BXPC 00000001)
[    0.014901] No NUMA configuration found
[    0.014904] Faking a node at [mem 0x0000000000000000-0x000000003ffdffff]
[    0.014914] NODE_DATA(0) allocated [mem 0x3ffb5000-0x3ffdffff]
[    0.053193] Zone ranges:
[    0.053199]   DMA      [mem 0x0000000000001000-0x0000000000ffffff]
[    0.053202]   DMA32    [mem 0x0000000001000000-0x000000003ffdffff]
[    0.053204]   Normal   empty
[    0.053205]   Device   empty
[    0.053207] Movable zone start for each node
[    0.053210] Early memory node ranges
[    0.053212]   node   0: [mem 0x0000000000001000-0x000000000009efff]
[    0.053214]   node   0: [mem 0x0000000000100000-0x000000003ffdffff]
[    0.095524] Zeroed struct page in unavailable ranges: 98 pages
[    0.095528] Initmem setup node 0 [mem 0x0000000000001000-0x000000003ffdffff]
[    0.457909] ACPI: PM-Timer IO Port: 0x608
[    0.457944] ACPI: LAPIC_NMI (acpi_id[0xff] dfl dfl lint[0x1])
[    0.458136] IOAPIC[0]: apic_id 0, version 17, address 0xfec00000, GSI 0-23
[    0.458142] ACPI: INT_SRC_OVR (bus 0 bus_irq 0 global_irq 2 dfl dfl)
[    0.458144] ACPI: INT_SRC_OVR (bus 0 bus_irq 5 global_irq 5 high level)
[    0.458145] ACPI: INT_SRC_OVR (bus 0 bus_irq 9 global_irq 9 high level)
[    0.458146] ACPI: INT_SRC_OVR (bus 0 bus_irq 10 global_irq 10 high level)
[    0.458147] ACPI: INT_SRC_OVR (bus 0 bus_irq 11 global_irq 11 high level)
[    0.458153] Using ACPI (MADT) for SMP configuration information
[    0.458156] smpboot: Allowing 2 CPUs, 0 hotplug CPUs
[    0.458216] PM: Registered nosave memory: [mem 0x00000000-0x00000fff]
[    0.458218] PM: Registered nosave memory: [mem 0x0009f000-0x0009ffff]
[    0.458219] PM: Registered nosave memory: [mem 0x000a0000-0x000effff]
[    0.458219] PM: Registered nosave memory: [mem 0x000f0000-0x000fffff]
[    0.458221] [mem 0x40000000-0xfeffbfff] available for PCI devices
[    0.458223] Booting paravirtualized kernel on KVM
[    0.458226] clocksource: refined-jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645519600211568 ns
[    0.458231] setup_percpu: NR_CPUS:8192 nr_cpumask_bits:2 nr_cpu_ids:2 nr_node_ids:1
[    0.510896] percpu: Embedded 54 pages/cpu s184320 r8192 d28672 u1048576
[    0.510973] KVM setup async PF for cpu 0
[    0.511000] kvm-stealtime: cpu 0, msr 3ea2c040
[    0.511028] PV qspinlock hash table entries: 256 (order: 0, 4096 bytes, linear)
[    0.511048] Built 1 zonelists, mobility grouping on.  Total pages: 257897
[    0.511049] Policy zone: DMA32
[    0.511051] Kernel command line: LABEL=cirros-rootfs ro console=tty1 console=ttyS0
[    0.511902] Dentry cache hash table entries: 131072 (order: 8, 1048576 bytes, linear)
[    0.512311] Inode-cache hash table entries: 65536 (order: 7, 524288 bytes, linear)
[    0.512422] mem auto-init: stack:off, heap alloc:on, heap free:off
[    0.515461] Memory: 989852K/1048056K available (14339K kernel code, 2370K rwdata, 4668K rodata, 2660K init, 5076K bss, 58204K reserved, 0K cma-reserved)
[    0.515471] random: get_random_u64 called from __kmem_cache_create+0x41/0x540 with crng_init=0
[    0.555913] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=2, Nodes=1
[    0.555950] Kernel/User page tables isolation: enabled
[    0.556068] ftrace: allocating 42927 entries in 168 pages
[    0.602653] rcu: Hierarchical RCU implementation.
[    0.602657] rcu: 	RCU restricting CPUs from NR_CPUS=8192 to nr_cpu_ids=2.
[    0.602659] 	Tasks RCU enabled.
[    0.602660] rcu: RCU calculated value of scheduler-enlistment delay is 25 jiffies.
[    0.602661] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=2
[    0.605819] NR_IRQS: 524544, nr_irqs: 440, preallocated irqs: 16
[    0.684923] Console: colour VGA+ 80x25
[    0.801907] printk: console [tty1] enabled
[    1.338938] printk: console [ttyS0] enabled
[    1.342677] ACPI: Core revision 20190703
[    1.346491] APIC: Switch to symmetric I/O mode setup
[    1.352914] x2apic enabled
[    1.357122] Switched APIC routing to physical x2apic.
[    1.365592] ..TIMER: vector=0x30 apic1=0 pin1=2 apic2=-1 pin2=-1
[    1.370742] clocksource: tsc-early: mask: 0xffffffffffffffff max_cycles: 0x229833f6470, max_idle_ns: 440795327230 ns
[    1.378978] Calibrating delay loop (skipped) preset value.. 4799.99 BogoMIPS (lpj=9599984)
[    1.382980] pid_max: default: 32768 minimum: 301
[    1.390998] LSM: Security Framework initializing
[    1.395001] Yama: becoming mindful.
[    1.399030] AppArmor: AppArmor initialized
[    1.403028] Mount-cache hash table entries: 2048 (order: 2, 16384 bytes, linear)
[    1.406994] Mountpoint-cache hash table entries: 2048 (order: 2, 16384 bytes, linear)
[    1.415208] *** VALIDATE proc ***
[    1.419056] *** VALIDATE cgroup1 ***
[    1.422978] *** VALIDATE cgroup2 ***
[    1.427793] Last level iTLB entries: 4KB 0, 2MB 0, 4MB 0
[    1.430978] Last level dTLB entries: 4KB 0, 2MB 0, 4MB 0, 1GB 0
[    1.434983] Spectre V1 : Mitigation: usercopy/swapgs barriers and __user pointer sanitization
[    1.438984] Spectre V2 : Mitigation: Full generic retpoline
[    1.442980] Spectre V2 : Spectre v2 / SpectreRSB mitigation: Filling RSB on context switch
[    1.446979] Speculative Store Bypass: Vulnerable
[    1.450983] MDS: Vulnerable: Clear CPU buffers attempted, no microcode
[    1.467098] Freeing SMP alternatives memory: 40K
[    1.582716] smpboot: CPU0: Intel QEMU Virtual CPU version 2.5+ (family: 0x6, model: 0xd, stepping: 0x3)
[    1.583290] Performance Events: PMU not available due to virtualization, using software events only.
[    1.591151] rcu: Hierarchical SRCU implementation.
[    1.595941] NMI watchdog: Perf NMI watchdog permanently disabled
[    1.603104] smp: Bringing up secondary CPUs ...
[    1.611472] x86: Booting SMP configuration:
[    1.614985] .... node  #0, CPUs:      #1
[    0.756749] kvm-clock: cpu 1, msr 13c01041, secondary cpu clock
[    0.756749] smpboot: CPU 1 Converting physical 0 to logical die 1
[    1.631032] KVM setup async PF for cpu 1
[    1.634617] kvm-stealtime: cpu 1, msr 3eb2c040
[    1.638986] smp: Brought up 1 node, 2 CPUs
[    1.643001] smpboot: Max logical packages: 2
[    1.646983] smpboot: Total of 2 processors activated (9599.98 BogoMIPS)
[    1.655302] devtmpfs: initialized
[    1.659086] x86/mm: Memory block size: 128MB
[    1.663301] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
[    1.667019] futex hash table entries: 512 (order: 3, 32768 bytes, linear)
[    1.671184] pinctrl core: initialized pinctrl subsystem
[    1.675683] PM: RTC time: 20:37:28, date: 2022-03-20
[    1.679326] NET: Registered protocol family 16
[    1.687092] audit: initializing netlink subsys (disabled)
[    1.691066] audit: type=2000 audit(1647808648.511:1): state=initialized audit_enabled=0 res=1
[    1.691219] EISA bus registered
[    1.698998] cpuidle: using governor ladder
[    1.702988] cpuidle: using governor menu
[    1.707219] ACPI: bus type PCI registered
[    1.710992] acpiphp: ACPI Hot Plug PCI Controller Driver version: 0.5
[    1.719167] PCI: Using configuration type 1 for base access
[    1.724660] HugeTLB registered 2.00 MiB page size, pre-allocated 0 pages
[    1.739241] ACPI: Added _OSI(Module Device)
[    1.743016] ACPI: Added _OSI(Processor Device)
[    1.746995] ACPI: Added _OSI(3.0 _SCP Extensions)
[    1.750978] ACPI: Added _OSI(Processor Aggregator Device)
[    1.755048] ACPI: Added _OSI(Linux-Dell-Video)
[    1.759104] ACPI: Added _OSI(Linux-Lenovo-NV-HDMI-Audio)
[    1.762991] ACPI: Added _OSI(Linux-HPI-Hybrid-Graphics)
[    1.767992] ACPI: 1 ACPI AML tables successfully acquired and loaded
[    1.773021] ACPI: Interpreter enabled
[    1.774996] ACPI: (supports S0 S5)
[    1.778981] ACPI: Using IOAPIC for interrupt routing
[    1.783024] PCI: Using host bridge windows from ACPI; if necessary, use "pci=nocrs" and report a bug
[    1.787218] ACPI: Enabled 2 GPEs in block 00 to 0F
[    1.799114] ACPI: PCI Root Bridge [PCI0] (domain 0000 [bus 00-ff])
[    1.802997] acpi PNP0A03:00: _OSC: OS supports [ASPM ClockPM Segments MSI HPX-Type3]
[    1.807004] acpi PNP0A03:00: fail to add MMCONFIG information, can't access extended PCI configuration space under this bridge.
[    1.811582] acpiphp: Slot [3] registered
[    1.815058] acpiphp: Slot [4] registered
[    1.819101] acpiphp: Slot [5] registered
[    1.823098] acpiphp: Slot [6] registered
[    1.827062] acpiphp: Slot [7] registered
[    1.831053] acpiphp: Slot [8] registered
[    1.835060] acpiphp: Slot [9] registered
[    1.839091] acpiphp: Slot [10] registered
[    1.843097] acpiphp: Slot [11] registered
[    1.847097] acpiphp: Slot [12] registered
[    1.851093] acpiphp: Slot [13] registered
[    1.855050] acpiphp: Slot [14] registered
[    1.859050] acpiphp: Slot [15] registered
[    1.863090] acpiphp: Slot [16] registered
[    1.867044] acpiphp: Slot [17] registered
[    1.871098] acpiphp: Slot [18] registered
[    1.875094] acpiphp: Slot [19] registered
[    1.879060] acpiphp: Slot [20] registered
[    1.883066] acpiphp: Slot [21] registered
[    1.887050] acpiphp: Slot [22] registered
[    1.891046] acpiphp: Slot [23] registered
[    1.895052] acpiphp: Slot [24] registered
[    1.899059] acpiphp: Slot [25] registered
[    1.903070] acpiphp: Slot [26] registered
[    1.907054] acpiphp: Slot [27] registered
[    1.911055] acpiphp: Slot [28] registered
[    1.915051] acpiphp: Slot [29] registered
[    1.919049] acpiphp: Slot [30] registered
[    1.923054] acpiphp: Slot [31] registered
[    1.927072] PCI host bridge to bus 0000:00
[    1.930987] pci_bus 0000:00: root bus resource [io  0x0000-0x0cf7 window]
[    1.934988] pci_bus 0000:00: root bus resource [io  0x0d00-0xffff window]
[    1.938984] pci_bus 0000:00: root bus resource [mem 0x000a0000-0x000bffff window]
[    1.942987] pci_bus 0000:00: root bus resource [mem 0x40000000-0xfebfffff window]
[    1.946984] pci_bus 0000:00: root bus resource [mem 0x100000000-0x17fffffff window]
[    1.950987] pci_bus 0000:00: root bus resource [bus 00-ff]
[    1.955205] pci 0000:00:00.0: [8086:1237] type 00 class 0x060000
[    1.963530] pci 0000:00:01.0: [8086:7000] type 00 class 0x060100
[    1.973056] pci 0000:00:01.1: [8086:7010] type 00 class 0x010180
[    1.986988] pci 0000:00:01.1: reg 0x20: [io  0xc040-0xc04f]
[    1.997009] pci 0000:00:01.1: legacy IDE quirk: reg 0x10: [io  0x01f0-0x01f7]
[    1.998987] pci 0000:00:01.1: legacy IDE quirk: reg 0x14: [io  0x03f6]
[    2.003004] pci 0000:00:01.1: legacy IDE quirk: reg 0x18: [io  0x0170-0x0177]
[    2.006986] pci 0000:00:01.1: legacy IDE quirk: reg 0x1c: [io  0x0376]
[    2.015484] pci 0000:00:01.3: [8086:7113] type 00 class 0x068000
[    2.025488] pci 0000:00:01.3: quirk: [io  0x0600-0x063f] claimed by PIIX4 ACPI
[    2.027047] pci 0000:00:01.3: quirk: [io  0x0700-0x070f] claimed by PIIX4 SMB
[    2.036752] pci 0000:00:02.0: [1234:1111] type 00 class 0x030000
[    2.044146] pci 0000:00:02.0: reg 0x10: [mem 0xfd000000-0xfdffffff pref]
[    2.055158] pci 0000:00:02.0: reg 0x18: [mem 0xfebf0000-0xfebf0fff]
[    2.087131] pci 0000:00:02.0: reg 0x30: [mem 0xfebe0000-0xfebeffff pref]
[    2.097746] pci 0000:00:03.0: [8086:100e] type 00 class 0x020000
[    2.110984] pci 0000:00:03.0: reg 0x10: [mem 0xfebc0000-0xfebdffff]
[    2.126988] pci 0000:00:03.0: reg 0x14: [io  0xc000-0xc03f]
[    2.150985] pci 0000:00:03.0: reg 0x30: [mem 0xfeb80000-0xfebbffff pref]
[    2.213221] ACPI: PCI Interrupt Link [LNKA] (IRQs 5 *10 11)
[    2.219344] ACPI: PCI Interrupt Link [LNKB] (IRQs 5 *10 11)
[    2.223328] ACPI: PCI Interrupt Link [LNKC] (IRQs 5 10 *11)
[    2.231341] ACPI: PCI Interrupt Link [LNKD] (IRQs 5 10 *11)
[    2.235144] ACPI: PCI Interrupt Link [LNKS] (IRQs *9)
[    2.243486] SCSI subsystem initialized
[    2.247186] pci 0000:00:02.0: vgaarb: setting as boot VGA device
[    2.250981] pci 0000:00:02.0: vgaarb: VGA device added: decodes=io+mem,owns=io+mem,locks=none
[    2.258986] pci 0000:00:02.0: vgaarb: bridge control possible
[    2.266979] vgaarb: loaded
[    2.267069] ACPI: bus type USB registered
[    2.271023] usbcore: registered new interface driver usbfs
[    2.275008] usbcore: registered new interface driver hub
[    2.283033] usbcore: registered new device driver usb
[    2.287123] pps_core: LinuxPPS API ver. 1 registered
[    2.290978] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    2.298991] PTP clock support registered
[    2.303081] EDAC MC: Ver: 3.0.0
[    2.307770] PCI: Using ACPI for IRQ routing
[    2.318978] NetLabel: Initializing
[    2.322980] NetLabel:  domain hash size = 128
[    2.326978] NetLabel:  protocols = UNLABELED CIPSOv4 CALIPSO
[    2.331046] NetLabel:  unlabeled traffic allowed by default
[    2.339332] clocksource: Switched to clocksource kvm-clock
[    2.533821] VFS: Disk quotas dquot_6.6.0
[    2.537850] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
[    2.544154] *** VALIDATE hugetlbfs ***
[    2.548152] AppArmor: AppArmor Filesystem Enabled
[    2.552588] pnp: PnP ACPI init
[    2.556737] pnp: PnP ACPI: found 5 devices
[    2.562710] thermal_sys: Registered thermal governor 'fair_share'
[    2.562711] thermal_sys: Registered thermal governor 'bang_bang'
[    2.568105] thermal_sys: Registered thermal governor 'step_wise'
[    2.573537] thermal_sys: Registered thermal governor 'user_space'
[    2.579008] thermal_sys: Registered thermal governor 'power_allocator'
[    2.589207] clocksource: acpi_pm: mask: 0xffffff max_cycles: 0xffffff, max_idle_ns: 2085701024 ns
[    2.603921] pci_bus 0000:00: resource 4 [io  0x0000-0x0cf7 window]
[    2.609112] pci_bus 0000:00: resource 5 [io  0x0d00-0xffff window]
[    2.614460] pci_bus 0000:00: resource 6 [mem 0x000a0000-0x000bffff window]
[    2.621137] pci_bus 0000:00: resource 7 [mem 0x40000000-0xfebfffff window]
[    2.628042] pci_bus 0000:00: resource 8 [mem 0x100000000-0x17fffffff window]
[    2.635923] NET: Registered protocol family 2
[    2.640335] tcp_listen_portaddr_hash hash table entries: 512 (order: 1, 8192 bytes, linear)
[    2.648365] TCP established hash table entries: 8192 (order: 4, 65536 bytes, linear)
[    2.655064] TCP bind hash table entries: 8192 (order: 5, 131072 bytes, linear)
[    2.661057] TCP: Hash tables configured (established 8192 bind 8192)
[    2.666834] UDP hash table entries: 512 (order: 2, 16384 bytes, linear)
[    2.672661] UDP-Lite hash table entries: 512 (order: 2, 16384 bytes, linear)
[    2.679179] NET: Registered protocol family 1
[    2.683898] NET: Registered protocol family 44
[    2.688046] pci 0000:00:01.0: PIIX3: Enabling Passive Release
[    2.693184] pci 0000:00:00.0: Limiting direct PCI/PCI transfers
[    2.698378] pci 0000:00:01.0: Activating ISA DMA hang workarounds
[    2.704995] pci 0000:00:02.0: Video device with shadowed ROM at [mem 0x000c0000-0x000dffff]
[    2.711990] PCI: CLS 0 bytes, default 64
[    2.715851] Trying to unpack rootfs image as initramfs...
[    2.970662] Freeing initrd memory: 6396K
[    2.974940] clocksource: tsc: mask: 0xffffffffffffffff max_cycles: 0x229833f6470, max_idle_ns: 440795327230 ns
[    2.983640] check: Scanning for low memory corruption every 60 seconds
[    2.989856] Initialise system trusted keyrings
[    2.993971] Key type blacklist registered
[    2.997700] workingset: timestamp_bits=36 max_order=18 bucket_order=0
[    3.005737] zbud: loaded
[    3.009256] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    3.014796] fuse: init (API version 7.31)
[    3.018579] Platform Keyring initialized
[    3.028164] Key type asymmetric registered
[    3.032123] Asymmetric key parser 'x509' registered
[    3.036510] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 244)
[    3.043586] io scheduler mq-deadline registered
[    3.048145] shpchp: Standard Hot Plug PCI Controller Driver version: 0.4
[    3.054368] input: Power Button as /devices/LNXSYSTM:00/LNXPWRBN:00/input/input0
[    3.060707] ACPI: Power Button [PWRF]
[    3.065077] Serial: 8250/16550 driver, 32 ports, IRQ sharing enabled
[    3.102462] 00:04: ttyS0 at I/O 0x3f8 (irq = 4, base_baud = 115200) is a 16550A
[    3.110770] Linux agpgart interface v0.103
[    3.127788] loop: module loaded
[    3.132969] scsi host0: ata_piix
[    3.136370] scsi host1: ata_piix
[    3.139603] ata1: PATA max MWDMA2 cmd 0x1f0 ctl 0x3f6 bmdma 0xc040 irq 14
[    3.145205] ata2: PATA max MWDMA2 cmd 0x170 ctl 0x376 bmdma 0xc048 irq 15
[    3.151513] libphy: Fixed MDIO Bus: probed
[    3.155725] tun: Universal TUN/TAP device driver, 1.6
[    3.161654] PPP generic driver version 2.4.2
[    3.165740] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    3.171772] ehci-pci: EHCI PCI platform driver
[    3.175802] ehci-platform: EHCI generic platform driver
[    3.180389] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
[    3.185746] ohci-pci: OHCI PCI platform driver
[    3.189771] ohci-platform: OHCI generic platform driver
[    3.194845] uhci_hcd: USB Universal Host Controller Interface driver
[    3.208850] i8042: PNP: PS/2 Controller [PNP0303:KBD,PNP0f13:MOU] at 0x60,0x64 irq 1,12
[    3.218449] serio: i8042 KBD port at 0x60,0x64 irq 1
[    3.223031] serio: i8042 AUX port at 0x60,0x64 irq 12
[    3.228096] mousedev: PS/2 mouse device common for all mice
[    3.234119] input: AT Translated Set 2 keyboard as /devices/platform/i8042/serio0/input/input1
[    3.242013] rtc_cmos 00:00: RTC can wake from S4
[    3.248677] rtc_cmos 00:00: registered as rtc0
[    3.253570] rtc_cmos 00:00: alarms up to one day, y3k, 114 bytes nvram
[    3.261277] i2c /dev entries driver
[    3.265006] device-mapper: uevent: version 1.0.3
[    3.270734] device-mapper: ioctl: 4.40.0-ioctl (2019-01-18) initialised: dm-devel@redhat.com
[    3.279609] platform eisa.0: Probing EISA bus 0
[    3.283780] platform eisa.0: EISA: Cannot allocate resource for mainboard
[    3.289635] platform eisa.0: Cannot allocate resource for EISA slot 1
[    3.295440] platform eisa.0: Cannot allocate resource for EISA slot 2
[    3.301064] platform eisa.0: Cannot allocate resource for EISA slot 3
[    3.306835] platform eisa.0: Cannot allocate resource for EISA slot 4
[    3.315071] platform eisa.0: Cannot allocate resource for EISA slot 5
[    3.321086] platform eisa.0: Cannot allocate resource for EISA slot 6
[    3.322243] ata1.00: ATA-7: QEMU HARDDISK, 2.5+, max UDMA/100
[    3.326716] platform eisa.0: Cannot allocate resource for EISA slot 7
[    3.332598] ata1.00: 229376 sectors, multi 16: LBA48
[    3.344620] platform eisa.0: Cannot allocate resource for EISA slot 8
[    3.345720] ata2.00: ATAPI: QEMU DVD-ROM, 2.5+, max UDMA/100
[    3.350029] platform eisa.0: EISA: Detected 0 cards
[    3.357541] scsi 0:0:0:0: Direct-Access     ATA      QEMU HARDDISK    2.5+ PQ: 0 ANSI: 5
[    3.359788] intel_pstate: CPU model not supported
[    3.370125] sd 0:0:0:0: [sda] 229376 512-byte logical blocks: (117 MB/112 MiB)
[    3.381144] sd 0:0:0:0: Attached scsi generic sg0 type 0
[    3.381185] ledtrig-cpu: registered to indicate activity on CPUs
[    3.386926] sd 0:0:0:0: [sda] Write Protect is off
[    3.392945] NET: Registered protocol family 10
[    3.397632] sd 0:0:0:0: [sda] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
[    3.409404] scsi 1:0:0:0: CD-ROM            QEMU     QEMU DVD-ROM     2.5+ PQ: 0 ANSI: 5
[    3.430118]  sda: sda1 sda15
[    3.433668] sd 0:0:0:0: [sda] Attached SCSI disk
[    3.437729] Segment Routing with IPv6
[    3.441150] NET: Registered protocol family 17
[    3.445128] Key type dns_resolver registered
[    3.449957] RAS: Correctable Errors collector initialized.
[    3.458288] registered taskstats version 1
[    3.462259] Loading compiled-in X.509 certificates
[    3.462291] sr 1:0:0:0: [sr0] scsi3-mmc drive: 4x/4x cd/rw xa/form2 tray
[    3.475196] cdrom: Uniform CD-ROM driver Revision: 3.20
[    3.482060] sr 1:0:0:0: Attached scsi generic sg1 type 5
[    3.488133] Loaded X.509 cert 'Build time autogenerated kernel key: 8098f9b3401d48cb244b138af0c5bac131caef8a'
[    3.498206] zswap: loaded using pool lzo/zbud
[    3.509789] Key type big_key registered
[    3.520824] Key type encrypted registered
[    3.525056] AppArmor: AppArmor sha1 policy hashing enabled
[    3.529991] ima: No TPM chip found, activating TPM-bypass!
[    3.536232] ima: Allocated hash algorithm: sha1
[    3.540556] No architecture policies found
[    3.544372] evm: Initialising EVM extended attributes:
[    3.549119] evm: security.selinux
[    3.552487] evm: security.SMACK64
[    3.556069] evm: security.SMACK64EXEC
[    3.559600] evm: security.SMACK64TRANSMUTE
[    3.563659] evm: security.SMACK64MMAP
[    3.568554] evm: security.apparmor
[    3.572377] evm: security.ima
[    3.575472] evm: security.capability
[    3.578895] evm: HMAC attrs: 0x1
[    3.582638] PM:   Magic number: 14:188:650
[    3.586797] tty ttyprintk: hash matches
[    3.591574] rtc_cmos 00:00: setting system clock to 2022-03-20T20:37:30 UTC (1647808650)
[    3.600360] Unstable clock detected, switching default tracing clock to "global"
[    3.600360] If you want to keep using the local clock, then add:
[    3.600360]   "trace_clock=local"
[    3.600360] on the kernel command line
[    3.622854] Freeing unused decrypted memory: 2040K
[    3.629449] Freeing unused kernel image memory: 2660K
[    3.635370] Write protecting the kernel read-only data: 22528k
[    3.643055] Freeing unused kernel image memory: 2008K
[    3.649034] Freeing unused kernel image memory: 1476K
[    3.662479] x86/mm: Checked W+X mappings: passed, no W+X pages found.
[    3.669641] x86/mm: Checking user space page tables
[    3.685471] x86/mm: Checked W+X mappings: passed, no W+X pages found.
[    3.691987] Run /init as init process

info: initramfs: up at 2.95
currently loaded modules: 8139cp 8390 9pnet 9pnet_virtio ahci drm drm_kms_helper e1000 failover fb_sys_fops hid hid_generic ip_tables isofs libahci mii ne2k_pci net_failov
info: initramfs loading root from /dev/sda1
info: /etc/init.d/rc.sysinit: up at 4.05
info: container: none
currently loaded modules: 8139cp 8390 9pnet 9pnet_virtio ahci drm drm_kms_helper e1000 failover fb_sys_fops hid hid_generic ip_tables isofs libahci mii ne2k_pci net_failov
Initializing random number generator... [    4.893892] random: dd: uninitialized urandom read (512 bytes read)
done.
Starting acpid: OK
[    5.023367] random: mktemp: uninitialized urandom read (6 bytes read)
[    5.098751] random: sed: uninitialized urandom read (6 bytes read)
GROWROOT: NOCHANGE: partition 1 is size 210911. it cannot be grown
resize-rootfs already run per once
  ____               ____  ____
 / __/ __ ____ ____ / __ \/ __/
/ /__ / // __// __// /_/ /\ \
\___//_//_/  /_/   \____/___/
   http://cirros-cloud.net


login as 'cirros' user. default password: 'gocubsgo'. use 'sudo' for root.
cirros login:

```














---
