# Onyx USB C Option Card (Unofficial FireWire Slot USB-C Retrofit Upgrade)  

What is?
A drop-in USB-C Audio Class 2 replacement card for legacy Mackie Onyx 1220, 1620, and 1640 FireWire expansion slots.

Why do?
The original Onyx live mixers endeavoured to set themselves apart by providing an option to add a Mackie Onyx FireWire I/O Card into already packed chassis. Firewire in 2026 is essentially lost to us in any meaningful way, so I wanted to develop a way to bring the Mackie Onyx 1st Generation Live Mixers which includes the first generation of the Onyx lineup of mixers labeled Onyx-1220, Onyx-1620, Onyx-1640.

I lucked into a great used option on EBAY and now I am a proud Mackie Onyx-1220 with the included Firewire Option Card (originally listed with the clever sku ONYXFIREWCARD). This lead me to wandering and wondering... how did Mackie set up the signal chain in order to get the sound into a digitally transmittable form with just an addon card. They must have had to do all the analogue to digital conversion on the card itself. And bingo, that's exactly how they did it I believe. If I'm right then it means that the leads from the internal ribbon cable are sending raw signal to the firewire card. This means I should be able to throw this on an oscilloscope to learn something or other or two somethings or others.

What now?
As I understand it currently after research the main mixer chassis routes raw analog channel taps directly to the backplane header. This means I should be able to build a modern multi-channel ADC frontend and pair it with a USB Audio Class 2 (UAC2) controller.

Buildout theory for a Custom USB-C Module
Header Pinout & Signal Mapping 
Analog Inputs: Individual balanced or single-ended analog taps coming off the 1220 channel buses (post-gain/EQ taps).
Power Rails: Bipolar analog rails (typically +/-15V) and digital logic rails (+5V/+3.3V) supplied by the main PSU. 
Control Lines: Ground references, chassis ground, and any routing status sensing pins. 

ADC Pipeline 
Use a multi-channel 24-bit / 192kHz ADC chip (Cirrus Logic CS5368 (8-channel) or AKM AK5388) to take the analog header lines and convert them into standard I2S/TDM digital audio streams. Implement ultra-low-noise LDO regulators (e.g., Texas Instruments TPS7A series) to step down the mixer's power rails specifically for the analog reference voltages on the converters to maintain high dynamic range. 

USB Audio Class 2 Bridge 
MCU / DSP Engine: An XMOS xCORE-200 series (e.g. XU216 or an STM32H7) configured for multi-channel USB Audio Class 2. 
Plug-and-Play Compatibility: Native driverless operation on macOS (CoreAudio) and Linux (ALSA), with standard UAC2 ASIO drivers on Windows.
PHY: USB 2.0 High-Speed (480 Mbps) PHY feeding a ESD-protected USB-C receptacle.

Board Layout & Mechanical
Match the original rear cutout dimensions and mounting standoff locations for a clean flush mount. Keep digital switching circuits strictly isolated from the analog input traces coming off the ribbon header to avoid introducing noise into the preamps.
