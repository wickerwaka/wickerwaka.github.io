+++
title = "Taito F2"
date = "2023-06-02T06:20:52-07:00"
description = "I have decided that I'm going to reverse engineer the Taito F2 arcade system and develop a MiSTer FPGA core for it."
+++

Hi! I have decided that I'm going to reverse engineer the Taito F2 arcade system and develop a MiSTer FPGA core for it.

The F2 is an arcade system developed by Taito and used for over a dozen games released between 1988 and 1993. It is based around a Motorola 68000 as it's main CPU and a Zilog Z80 for audio, a very popular pairing at the time. I have been a fan of both of these processors for a long time since the Z80 based ZX Spectrum and 68000 based Atari ST were my first computers. For me, these processors define 8-bit and 16-bit computing.

Unlike the IREM M72 and M92 systems (which are the only other arcade boards I have experience with) the Taito F2 uses very few discreet logic ICs. The system uses a collection of custom ICs. Some of those customs are common to all games and the rest are used based on the needs of each game.

Games come in both single board and dual board form factors. In the dual board form factor the "Mother PCB" contains the CPUs, sound hardware, most of the RAM and the common custom ICs that handle sprite and tilemap drawing. The "Game PCB" contains the ROMs and any custom ICs the game needs.

I currently own a single board *Final Blow* PCB. It is the earliest F2 game as well as one of the cheapest and most readily available. Most importantly there are also [schematics](https://www.arcade-museum.com/manuals-videogames/F/FinalBlow.sch.pdf) available for it. Really nice schematics actually. They are clearly labeled and provide a lot of valuable information about the custom ICs.

My plan is to create a test ROM similar to the one I built for the M92 boards. This ROM will allow me to send test signals to the custom ICs and analyze the results with a scope, logic analyzer or just "looking at the screen." Later on this test ROM will also be useful for validating core behavior vs original hardware.

