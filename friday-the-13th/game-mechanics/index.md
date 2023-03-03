---
title: Friday the 13th - Game Mechanics
layout: default
nav_order: 3
parent: Friday the 13th
---

Some general notes on game mechanics with occasional pointers to memory locations. I've got a bunch of half-figured out stuff and will be updating this as I get time to write it up.

## Day/Night Cycle

The game progresses through day, dusk, and night modes over the course of three days.
Transitions from day to dusk and dusk to night are determined by how many times the counselor exits a cabin (tracked in RAM 0x523). When that value hits 0x05 and 0x08, the game goes to dusk and night (see code near 0xBD72).

Night ends when Jason gets knocked-out (see code near 0x8E20).

Among many things, the Day/Night cycle affects the maximum number of enemies on screen (code 0xDAF8).
