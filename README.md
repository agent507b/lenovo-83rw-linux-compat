Root cause analysis
Secure Boot context
Device purchased as No-OS/FreeDOS — Secure Boot is disabled. This results in an unsigned boot environment that may block critical ACPI power-management exchanges required by the Panther Lake display engine.
Observed impact:

The embedded controller (EC) on the 83RW appears to bypass the PWM signal from the Intel Xe driver when Secure Boot is off
Switching to discrete graphics mode (Secure Boot off) causes immediate display corruption — the Nvidia VBIOS cannot negotiate eDP link training nor backlight routing without the secure exchange expected by T4CN38WW firmware

Conclusion
Complete driver isolation confirmed via /proc/cmdline. Writing to /sys/class/backlight/intel_backlight/brightness updates system state — physical PWM duty cycle remains locked at 100% on the eDP panel.
The Intel Xe driver is writing to a register that the 83RW firmware does not route to the panel's backlight controller.
This is a VBT (Video BIOS Table) or EC mapping error specific to model 83RW under Linux.

Requested action
Escalation to BIOS/Engineering team required.
A UEFI update is needed to:

Properly expose ACPI backlight methods (_BCL, _BCM)
Fix VBIOS handoff stability for the Panther Lake platform on Legion 5 (83RW)


Status
DateEvent2026-01Report submitted to Lenovo—Awaiting BIOS update
