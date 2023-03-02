---
title: Friday the 13th - ROM Map
layout: default
nav_order: 2
parent: Friday the 13th
---

| Address | Name | Context       |Notes | Values |
| :---:   | :--- | :---          | :--- |  :---      |
| 0x505   | Player Health    | Inside cabin. |  UI doesn't auto update    |   TBD  |
| 0x507   | Active Counselor | Map Screen |  Index value identifying counselor  |   TBD  |
| 0x51C   | Jason Health     | Inside cabin. |  UI doesn't auto update    |   TBD  |

# ROM Map

## Counselor Attributes
| Address   | Description               | Notes |
| :---      | :---                      | :---  |
| 0x8AEB | Counselor names              | 36 bytes total, fixed width 6 bytes per name. See text table for char values. <br />0xFF denotes blank character.|
| 0xB268 | Counselor walk speed         | 6 bytes, indexed by counselor id. <br />0x03 = daytime zombie speed<br />0x04 = normal<br />0x05 = fast<br />  0x06 = very fast<br /> 0x07 = ludicrous speed |
| 0xCC7E | Counselor min-kill for drops | 6 bytes, indexed by counselor id. Min number of mob kills for a counselor to get item drops   |

n.b. that counselor jump skill is at least partially hardcoded into the game.

## Misc Attributes
| Address   | Description             |
| :--    | :--             | 
| 0xDF2D | Table of weapon damage values |
| 0xDF4B | Table of mob health           |

## Zone information
| Address   | Description             |
| :--    | :--             | 
| 0x9806 | Length information about each zone?             |
| 0x9885 | 15 Bytes, either 0x00 or 0x01. Accessed by zone id. 0x01 associated with cave and forest zones |


## Jason Patrol 
| Address   | Description             |
| :--    | :--             | 
| 0xB62F | Multi-byte list containing Jason's patrol route |

Most of it is a sequence of 2-byte pairs. If the first byte is 0xFF, that's a special case.

Byte 1 & 0xF0: Id of zone 

Byte 1 & 0x0F: 'Section' of zone. Sectioning of zones is TBD. There's logic in the game (0xBA3A) which determines if the counselor meets Jason in the same section which can be used to flesh this out.

Byte 2 & 0x1F: ID of cabin Jason will enter (or 0 if no cabin)

Byte 2 & 0xE0: Dwell time of Jason at location (number of 0x50F ticks to countdown)

The major pointer into this address is the 0x50D-E pair.

When first entering the game proper, the frame counter 0x31 is used to 'randomly' initialize where 0x50D-E initially points.

That pointer is updated each time Jason moves.

If Byte 1 is 0xFF, then the next two bytes are used to update the value of the 0x50D-0x50E pointer.
