---
layout: post
title: "AppleIntelInfo.kext 使用"
modified: 2016-11-19 20:57:05 +0800
tags: [电源管理,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

我以前讲过如何用AppleIntelCPUPowerManagementInfo.kext来查看黑苹果电源管理是否正常工作，AppleIntelInfo.kext是AppleIntelCPUPowerManagementInfo.kext的替代者，并且，从其功能更加强大，收集的信息更加多样。使用如下：

下载编译[AppleIntelInfo.kext](https://github.com/Piker-Alpha/AppleIntelInfo)，生成AppleIntelInfo.kext。

{% highlight bash %}
sudo chown -R root:wheel AppleIntelInfo.kext
sudo kextutil AppleIntelInfo.kext
sudo cat /tmp/AppleIntelInfo.dat
{% endhighlight %}


以我的Toshiba Portege z30-b为例，信息如下：

{% highlight text %}

AppleIntelInfo.kext v2.0 Copyright © 2012-2016 Pike R. Alpha. All rights reserved
enableHWP................................: 0

Settings:
------------------------------------------
logMSRs..................................: 1
logIGPU..................................: 1
logCStates...............................: 1
logIPGStyle..............................: 1

Warning: Clover hw.busfrequency error detected : 17d78400
InitialTSC...............................: 0x1f7a2b22ebe4 (1504 MHz)
MWAIT C-States...........................: 286531872

Processor Brandstring....................: Intel(R) Core(TM) i5-5300U CPU @ 2.30GHz

Processor Signature..................... : 0x306D4
------------------------------------------
 - Family............................... : 6
 - Stepping............................. : 4
 - Model................................ : 0x3D (61)

Model Specific Registers (MSRs)
------------------------------------------

MSR_CORE_THREAD_COUNT............(0x35)  : 0x0
------------------------------------------
 - Core Count........................... : 2
 - Thread Count......................... : 4

MSR_PLATFORM_INFO................(0xCE)  : 0x5053BF3011700
------------------------------------------
 - Maximum Non-Turbo Ratio.............. : 0x17 (2300 MHz)
 - Ratio Limit for Turbo Mode........... : 1 (programmable)
 - TDP Limit for Turbo Mode............. : 1 (programmable)
 - Low Power Mode Support............... : 1 (LPM supported)
 - Number of ConfigTDP Levels........... : 1 (additional TDP level(s) available)
 - Maximum Efficiency Ratio............. : 5
 - Minimum Operating Ratio.............. : 5

MSR_PMG_CST_CONFIG_CONTROL.......(0xE2)  : 0x1E008406
------------------------------------------
 - I/O MWAIT Redirection Enable......... : 1 (enabled, IO read of MSR(0xE4) mapped to MWAIT)
 - CFG Lock............................. : 1 (MSR locked until next reset)
 - C3 State Auto Demotion............... : 1 (enabled)
 - C1 State Auto Demotion............... : 1 (enabled)
 - C3 State Undemotion.................. : 1 (enabled)
 - C1 State Undemotion.................. : 1 (enabled)
 - Package C-State Auto Demotion........ : 0 (disabled/unsupported)
 - Package C-State Undemotion........... : 0 (disabled/unsupported)

MSR_PMG_IO_CAPTURE_BASE..........(0xE4)  : 0x31814
------------------------------------------
 - LVL_2 Base Address................... : 0x1814

IA32_MPERF.......................(0xE7)  : 0x64113239192
IA32_APERF.......................(0xE8)  : 0x1DB6E4D7358

MSR_FLEX_RATIO...................(0x194) : 0x10000
------------------------------------------

MSR_IA32_PERF_STATUS.............(0x198) : 0x21BA00001B00
------------------------------------------
 - Current Performance State Value...... : 0x1B00 (2700 MHz)

MSR_IA32_PERF_CONTROL............(0x199) : 0x1D00
------------------------------------------
 - Target performance State Value....... : 0x1D00 (2900 MHz)
 - Intel Dynamic Acceleration........... : 0 (IDA engaged)

IA32_CLOCK_MODULATION............(0x19A) : 0x0

IA32_THERM_INTERRUPT.............(0x19B) : 0x0

IA32_THERM_STATUS................(0x19C) : 0x883D0808
------------------------------------------
 - Thermal Status....................... : 0
 - Thermal Log.......................... : 0
 - PROCHOT # or FORCEPR# event.......... : 0
 - PROCHOT # or FORCEPR# log............ : 1
 - Critical Temperature Status.......... : 0
 - Critical Temperature log............. : 0
 - Thermal Threshold #1 Status.......... : 0
 - Thermal Threshold #1 log............. : 0
 - Thermal Threshold #2 Status.......... : 0
 - Thermal Threshold #2 log............. : 0
 - Power Limitation Status.............. : 0
 - Power Limitation log................. : 1
 - Current Limit Status................. : 0
 - Current Limit log.................... : 0
 - Cross Domain Limit Status............ : 0
 - Cross Domain Limit log............... : 0
 - Digital Readout...................... : 61
 - Resolution in Degrees Celsius........ : 1
 - Reading Valid........................ : 1 (valid)

MSR_THERM2_CTL...................(0x19D) : 0x0

IA32_MISC_ENABLES................(0x1A0) : 0x850089
------------------------------------------
 - Fast-Strings......................... : 1 (enabled)
 - FOPCODE compatibility mode Enable.... : 0
 - Automatic Thermal Control Circuit.... : 1 (enabled)
 - Split-lock Disable................... : 0
 - Performance Monitoring............... : 1 (available)
 - Bus Lock On Cache Line Splits Disable : 0
 - Hardware prefetch Disable............ : 0
 - Processor Event Based Sampling....... : 0 (PEBS supported)
 - GV1/2 legacy Enable.................. : 0
 - Enhanced Intel SpeedStep Technology.. : 1 (enabled)
 - MONITOR FSM.......................... : 1 (MONITOR/MWAIT supported)
 - Adjacent sector prefetch Disable..... : 0
 - CFG Lock............................. : 0 (MSR not locked)
 - xTPR Message Disable................. : 1 (disabled)

MSR_TEMPERATURE_TARGET...........(0x1A2) : 0x690000
------------------------------------------
 - Turbo Attenuation Units.............. : 0 
 - Temperature Target................... : 105
 - TCC Activation Offset................ : 0

MSR_MISC_PWR_MGMT................(0x1AA) : 0x400001
------------------------------------------
 - EIST Hardware Coordination........... : 1 (hardware coordination disabled)
 - Energy/Performance Bias support...... : 1
 - Energy/Performance Bias.............. : 0 (disabled/MSR not visible to software)
 - Thermal Interrupt Coordination Enable : 1 (thermal interrupt routed to all cores)

MSR_TURBO_RATIO_LIMIT............(0x1AD) : 0x1B1B1B1B1B1D
------------------------------------------
 - Maximum Ratio Limit for C01.......... : 1D (2900 MHz) 
 - Maximum Ratio Limit for C02.......... : 1B (2700 MHz) 

IA32_ENERGY_PERF_BIAS............(0x1B0) : 0x5
------------------------------------------
 - Power Policy Preference...............: 5 (balanced performance and energy saving)

MSR_POWER_CTL....................(0x1FC) : 0x4005F
------------------------------------------
 - Bi-Directional Processor Hot..........: 1 (enabled)
 - C1E Enable............................: 1 (enabled)

MSR_RAPL_POWER_UNIT..............(0x606) : 0xA0E03
------------------------------------------
 - Power Units.......................... : 3 (1/8 Watt)
 - Energy Status Units.................. : 14 (61 micro-Joules)
 - Time Units .......................... : 10 (976.6 micro-Seconds)

MSR_PKG_POWER_LIMIT..............(0x610) : 0x4280C800DD8078
------------------------------------------
 - Package Power Limit #1............... : 15 Watt
 - Enable Power Limit #1................ : 1 (enabled)
 - Package Clamping Limitation #1....... : 1 (allow going below OS-requested P/T state during Time Window for Power Limit #1)
 - Time Window for Power Limit #1....... : 110 (163840 milli-Seconds)
 - Package Power Limit #2............... : 25 Watt
 - Enable Power Limit #2................ : 1 (enabled)
 - Package Clamping Limitation #2....... : 0 (disabled)
 - Time Window for Power Limit #2....... : 33 (10 milli-Seconds)
 - Lock................................. : 0 (MSR not locked)

MSR_PKG_ENERGY_STATUS............(0x611) : 0x2FE2B74E
------------------------------------------
 - Total Energy Consumed................ : 49034 Joules (Watt = Joules / seconds)

MSR_PKG_POWER_INFO...............(0x614) : 0x78
------------------------------------------
 - Thermal Spec Power................... : 15 Watt
 - Minimum Power........................ : 0
 - Maximum Power........................ : 0
 - Maximum Time Window.................. : 0

MSR_PP0_POWER_LIMIT..............(0x638) : 0x0

MSR_PP0_ENERGY_STATUS............(0x639) : 0x1F50F940
------------------------------------------
 - Total Energy Consumed................ : 32067 Joules (Watt = Joules / seconds)

MSR_TURBO_ACTIVATION_RATIO.......(0x64C) : 0x0

MSR_PKGC6_IRTL...................(0x60b) : 0x8873
MSR_PKGC7_IRTL...................(0x60c) : 0x8891
MSR_PKG_C2_RESIDENCY.............(0x60d) : 0x6AAB4D09D28
MSR_PKG_C3_RESIDENCY.............(0x3f8) : 0x15F1B404C1C
MSR_PKG_C6_RESIDENCY.............(0x3f9) : 0x4205FA89
MSR_PKG_C7_RESIDENCY.............(0x3fa) : 0xCD729046C1D

IA32_TSC_DEADLINE................(0x6E0) : 0x1F7A2E776809

CPU Ratio Info:
------------------------------------------
Base Clock Frequency (BLCK)............. : 100 MHz
Maximum Efficiency Ratio/Frequency.......:  5 ( 500 MHz)
Maximum non-Turbo Ratio/Frequency........: 23 (2300 MHz)
Maximum Turbo Ratio/Frequency............: 29 (2900 MHz)

IGPU Info:
------------------------------------------
IGPU Current Frequency...................:  350 MHz
IGPU Minimum Frequency...................:  300 MHz
IGPU Maximum Non-Turbo Frequency.........:  300 MHz
IGPU Maximum Turbo Frequency.............:  900 MHz
IGPU Maximum limit.......................: No Limit

P-State ratio * 100 = Frequency in MHz
------------------------------------------
CPU P-States [ (13) 19 27 ] iGPU P-States [ (7) ]
CPU C3-Cores [ 0 1 2 3 ]
CPU C6-Cores [ 0 1 2 3 ]
CPU C7-Cores [ 0 1 2 3 ]
CPU P-States [ (13) 14 19 27 ] iGPU P-States [ (7) ]
CPU P-States [ (13) 14 18 19 27 ] iGPU P-States [ 7 (9) ]
CPU P-States [ (13) 14 16 18 19 27 ] iGPU P-States [ (7) 9 ]
CPU P-States [ 13 14 15 16 18 19 (27) ] iGPU P-States [ (7) 9 ]
CPU P-States [ (13) 14 15 16 18 19 27 ] iGPU P-States [ 7 9 (18) ]
CPU P-States [ (13) 14 15 16 18 19 27 ] iGPU P-States [ 7 9 (14) 18 ]
CPU P-States [ 13 14 15 16 18 19 21 (25) 27 ] iGPU P-States [ 7 9 14 (18) ]
CPU P-States [ 13 14 15 16 18 19 21 25 (26) 27 ] iGPU P-States [ 7 9 14 (18) ]
CPU P-States [ (13) 14 15 16 17 18 19 21 25 26 27 ] iGPU P-States [ 7 9 (12) 14 18 ]
CPU P-States [ (13) 14 15 16 17 18 19 20 21 25 26 27 ] iGPU P-States [ (7) 9 12 14 18 ]
CPU P-States [ (13) 14 15 16 17 18 19 20 21 25 26 27 ] iGPU P-States [ 7 9 12 14 (16) 18 ]
CPU P-States [ 13 14 15 (16) 17 18 19 20 21 25 26 27 ] iGPU P-States [ 7 (8) 9 12 14 16 18 ]

{% endhighlight %}