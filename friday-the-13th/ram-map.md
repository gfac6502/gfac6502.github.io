---
title: Friday the 13th - RAM Map
layout: default
nav_order: 1
parent: Friday the 13th
---

N.b., the context for these RAM values is genrally during main gameplay (overworld, in cabins).  They may not be valid during the start screen, cinematics, etc.  

Some of this is definite, some is vague, and some serves as notes of suspected function. A '''?''' denotes that the there's more work to be done before I'm confident about the description.

# RAM Map

## Fast RAM (0x000-0x0FF)

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x000-0x001 | Often used to hold two-byte values for indirect addressing |                                                    |
| 0x31        | Frame counter                                                        | Used to 'randomly' pick where Jason begins pathing at game start |
| 0x65 | Min counselor x-loc? | Updated when encountering Jason on the road |
| 0x70 | Max counselor x-loc? |  Updated when encountering Jason on the road                                            |
| 0x8C | Something to do with length of current zone? |   |

## Stack (0x100-0x1FF)

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x100-0x1FF | Stack                                                                |                                                    |

## Tile Display (0x200-2FF)

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x200-0x2FF | Display info                                                         |                                                    |

Every 4 byte segment controls a tile displayed on screen.  
Byte:
1. Y-location (screen coordinates).  N.b. 0xFX is set to make it 'invisible' (*i.e.,* offscreen)
2. Tile index of graphic to display (*e.g.,* 0xB1 identifies a small cabin on the map screen)
3. Palette ID
4. X-location (screen coordinates)

## 0x300 - 0x35F

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x320       | Changes when a counselor projectile is airborne                      | 0x01 in air, otherwise 0x04                        |
| 0x328       | Changes when a counselor projectile is airborne                      | 0x46 in air, otherwise 0x2C                        |
| 0x329       | Changes when a counselor projectile hits zombie                      | Usually 0xBE, 0xFA during hit                      |
| 0x320       | Changes when a counselor projectile is airborne                      | 0x01 in air, otherwise 0x04                        |
| 0x340       | Changes when a counselor projectile is airborne                      | 0x00 in air, otherwise 0xC6                        |
| 0x348-0x349 | Timer active while counselor projectile is thrown.                   | Freezing value while fired doesn't allow second shot. Freeing before shot allows firing, but not a second shot .     |

## 0x360-0x3C0

This is an 8-byte wide table which tracks a bunch of entity attributes. Each column is an entity (player, mob, projectile) but not every column is appears used. Every row is is an attribute (x-location, attack state, pointers).  Listing linearly here, but I also plan to describe it as a table once I suss out more of the attributes.

Known columns: 
1. Counselor
2. counselor projectile
3. enemy #1 (includes Jason, probably Pamela)
4. enemy #2
5. enemy #3
6. Unknown/not used?
7. Jason projectile (probably other enemy projectiles)
8. Unknonw/not used?  Some non-0's, but appear placed during intro screen.

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x360       | x-location (screen coords) of counselor                              |                                                    |
| 0x361       | x-location (screen coords) of counselor projectile                   |                                                    |
| 0x369       | Tracks if counselor projectile is off-screen                         | 0x00 in air and visible, 0x01 goes off right screen, 0xFF off left. Value remains after stone disappears. Freezing at 0x01 allows throw animation, but not projectile. Freezing at 0x00 allows projectile to wrap around screen.                                                                                                                                     |
| 0x370       | y-location (screen coords) of counselor                              |                                                             |
| 0x371       | y-location (screen coords) of counselor projectile                   |                                                             | 
| 0x3C0       | Counselor walk direction (NOT facing direction)                       |           0x01 = Right, 0x02 = Left                         |   

## 0x3C1-0x3FF

TBD

## 0x400-4FF


| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x420       | Counselor move state                                                 |                                                    |

| Value     | Counselor move state                                                          | 
| :----       | :----                                                                       | 
| 0x00 | Standing/walking  |
| 0x01 | Jumping      |           
| 0x02 |  TBD          |          
| 0x03 | Crouching      |       
| 0x04 | Throwing        |          
| 0x05 | TBD              |      
| 0x06 | TBD              |
| 0x07 | Normal overworld death | 
| 0x08 | TBD                   |
| 0x09 | TBD                  |
| 0x0A | Walking in cabin     |
| 0xF0 | Death on boat         |

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x431       | Lifetime timer for counselor projectile        | Freezing allows projectile to continue until collision or going off screen. Freezing 0x369 as well allows projectile to wrap around.                |
| 0x482-0x484 | Damage done to mob 1,2, or 3 | For Jason, this goes to 6 and cycles back to 0 |

## 0x500-0x5FF

A lot of the game state is in this block, I've broken it into a few subsections for formatting.


| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x500 | ID of trail current counselor is on | 0x00 =  Lake loop                                        |                               |
|       |                                     | 0x01 = Cave 1 (D from entrance)                          | TODO add link to gamefaqs map |
|       |                                     | 0x02 = Cave 2 (A from entrance)                          | TODO add link to gamefaqs map |
|       |                                     | 0x03 = Cave 3 (Entrance)                                 |                               |
|       |                                     | 0x04 = On the lake (boating)                             |                               |
|       |                                     | 0x05 = Outer, largest loop                               |                               |
|       |                                     | 0x06 = Cave loop                                         |                               |
|       |                                     | 0x07 = Lower Forest South entrance                       |                               |
|       |                                     | 0x08 = Lower Forest d to F from cabin lane               |                               |
|       |                                     | 0x09 = Lower Forest North entrance                       |                               |
|       |                                     | 0x0A = Lower Forest c to D  from north entrance          |                               |
|       |                                     | 0x0B = Upper Forest Northern  entrance                   |                               |
|       |                                     | 0x0C = Upper Forest Cabin 1 lane (north entrance to d)   |                               |
|       |                                     | 0x0D = Upper Forest Southern entrance                    |                               |
|       |                                     | 0x0E = Upper Forest (h to E from south entrance)         |                               |
|       |                                     | 0x0F = Undefined, manually setting leads to glitch level |                               |


| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x501       | Associated with code near PPU read read via indirect addressing when changing screens  |                                  |
| 0x502       | Not sure. Initialized with 0x05 and read on screen changes. Stored in PPU. Countsdown during camper attack     |          |
| 0x503       | Number of remaining campers                                                                                    |          |
| 0x504       | Number of remaining counselors                                                                                 |          |
| 0x505       | Counselor Health                                                                                               |          |
| 0x506       | Weapon held by active counselor                                                                                |   TODO enumerate       |
| 0x507       | Identity of current counselor                                                                                  |          |
| 0x508       | Time of day                                                                                                    | 0x00=day, 0x01 = dusk, 0x03 = night   |
| 0x50B       | Second part of two-tier countdown (with 0x577) leading to camper attack                                        |          |
| 0x50C       | Compared with value of 2, to set bit 0x04 in 0x51F bitfield                                                    | While in cabins  |
| 0x50D-0x50E | Holds pointer to data defining Jason's walk path (ROM 0xB62F), set at 0xB8C7                                   | (See ROM map for details)                 |
| 0x50F       | Jason 'wait time' at current location                                                                          |                 |
| 0x510       | ID of trail Jason is on                                                                                        |                 |
| 0x511       | ID of attacked cabin                                                                                           |                 |
| 0x512       | Specific section of trail Jason is on                                                                          |                 |
| 0x513       | Number of campers in lake cabin B                                                                              |                 |
| 0x514       | Number of campers in lake cabin C                                                                              |                 |
| 0x515       | Number of campers in lake cabin D                                                                              |                 |
| 0x516       | Number of zombie (mob?) kills by current counselor                                                             |                 |
| 0x517       | Current counselor has lighter?                                                                                 | 0x01 = yes, 0x00 = no |
| 0x518       | Current counselor has flashlight?                                                                              | 0x01 = yes, 0x02 = no |
| 0x519       | Number of vitamins held by current counselor                                                                   |                 |
| 0x51A       | Current counselor has key?                                                                                     | 0x01 = yes, 0x00 = no |
| 0x51C       | Jason Health                                                                                                   |                       |
| 0x51F       | Bitfield tracking game state                                                                                   | 0x01 =  killed enough zombies for pickups <br /> 0x02 = Something to do with 2* min kill count <br /> 0x04 = triggered in cabins based on comparison between 0x50C and 2 <br /> 0x08 = set when entering a forest path <br />  0x10 set when enter a cave path <br /> 0x20 lit 4 fireplaces <br /> 0x40 set when picking up a knife <br /> set when picking up a key |   


| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x520       | Bitfield related to note reading                                                                               |       |
| 0x521       | Number of fireplaces lit                                                                                       |       |
| 0x522       | Number of times Jason knocked out                                                                              |       |
| 0x523       | Number of times cabin exited                                                                                   | Used to set time of day in 0x0508     |
| 0x568       | Has to do with mob and player positioning                                                                      |      |
| 0x570       | Varies with height during jumping if no zombie present. Freezing does weird things. Also possibly a countdown. |      |
| 0x576       | Is counselor projectile in air?                                                                                | 0x01 = yes |
| 0x577       | Top of two-tier countdown with 0x50B related to camper attacks                                                 |       |
| 0x57C-0x57D | Pointer  associated with lookups based on path and time of day                                                 |       |
| 0x580       | Appears to countdown                                                                                           |       |
| 0x587       | Part of Jason wait timer. Starts at 0x3C and if 0x58E is positive, counts down. At 0, 0x50F gets decremented   |       |
| 0x58A       |                                                                                                                |       |
| 0x58B       | Timer associated with camper attacks                                                                           |       |
| 0x58C       | Timer associated with camper attacks                                                                           | Plays part in killing off campers during attack interval    |
| 0x58D       | Camper attack countdown                                                                                        |       |
| 0x58E       | Has something to do with Jason activity/attacks.                                                               |       |                  | 0x590       | Some sort of countdown/sequence                                                                                |       |


| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x591 | ID of current cabin                               |  TODO draw up a map                                                         |

| Value     | Cabin                                      | 
| :----:       | :----                                   |
| 0x00      | None (verify?)                             |
| 0x01      | Bottom-central cabin on outer loop         |
| 0x02      | SE cabin on outer loop                     |
| 0x03      | NE Lake loop                               |
| 0x04      | NW Lake loop, small cabin                  |
| 0x05      | Mid-bottom lake loop cabin                 |
| 0x06      | SE lake loop cabin                         |
| 0x07      | NE Corner cabin                            |
| 0x08      | Most NW cabin, outer loop                  |
| 0x09      | SW cabin of cave loop                      |
| 0x0A      | NE cabin of cave loop                      |
| 0x0B      | Lake cabin 1                               |
| 0x0C      | Lake cabin 2                               |
| 0x0D      | Lake cabin 3                               |
| 0x0E      | First cabin CCW from north forest entrance |
| 0x0F      | NW cabin cave loop                         [|](|)
| 0x10      | SE cabin cave loop                                                 |
| 0x11      | SW cabin of of outer loop, first CW from cave trail                |
| 0x12      | SW lake loop cabin                                                 |
| 0x13      | NW lake loop big cabin, just after bottom of north forest entrance |
| 0x14      | ?                                                                  |
| 0x15      | South forest cabin                                                 |
| 0x16      | North Forest cabin 1                                               |
| 0x17      | North Forest cabin 2                                               |
| 0x18      | Cave cabin 1 (r d r u from entrance)                               |
| 0x19      | Cave cabin 2 (r u l u)                                             |
| 0x1A      | Cave cabin 3 (l u)                                                 |
| 0x1B-0x1F | Undefined                                                          |


| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x5A2 | Type of cabin room?                               | 0x19 = forest cabin door <br /> 0x23 = cave shrine, etc       |                             
| 0x5A3 | Cabin type? Index based on value of current cabin | 0x00 = normal cabin <br /> 0x01 = lakeside cabin <br /> 0x02 = torch cabin  <br /> 0x03 = forest cabin <br /> 0x04 = cave cabin |  
| 0x5AB | Cabin room in which Jason resides                 |                                                             |
| 0x592 | Fighting Jason in cabin? Jason in current cabin?         |                                                                    |   
| 0x593 | Jason is busy?                                           | When 1, no new Jason movement on map. Set when encountering Jason. |   
| 0x595 | Current number of active  enemies                        |                                                                    |   
| 0x596 | Maximum allowable active  enemies                        |                                                                    |   
| 0x5B1 | Number of times Jason has been hit in current encounter. | Runs away at 7                                                     |   
| 0x5B2 | Jason is busy?                                           | When 1, no new Jason movement on map.                              |   

## 0x600-0x6FF

| Address     | Description                                                          | Notes                                              |
| :----       | :----                                                                | :---                                               |
| 0x6B4-0x6EA | Item table for jump-triggered spawns |   |

Each byte corresponds to a section of the map where jumping can spawn items, provided the counselor has killed enough mobs.
Upper nibble is item type. Lower nibble is number of jumps to spawn.
On pickup, the value goes to 0x00.
The values are initialized when the main game loads. Notably, *from within chr rom data*.  ppu 0x14A7.


| Address     | Description                                                          | Notes                            |
| :----       | :----                                                                | :---                             |
| 0x6F4       | Is item 1 spawned?                                                   |                                  |
| 0x6F5       | Is item 2 spawned?                                                   |                                  |
| 0x6F6       | Spawned Item 1 identity      | 0x01 = knife, 0x07 = lighter, 0x09 = vitamins, 0x0A = key, 0x0C = machete. Others, at least up to 0xA9 give sprite junk |
| 0x6F7   | Spawned Item 2 identity      |                   |
| 0x6F8   | Xpos Spawned Item #1         |                   |
| 0x6F9   | Xpos Spawned Item #2         |                   |
| 0x6FA-B  | Not sure                     | Always 0, code analysis suggests so. Could be indirect addressed. Appears in code related to item spawn/pickup          |
| 0x6FC   | Ypos Spawned Item #1         |                   |
| 0x6FD   | Ypos Spawned Item #2         |                   |
| 0x695+Y | Is fireplace in cabin Y lit? |                   |

## 0x700-0x7FF

| Address     | Description                                                          | Notes                            |
| :----       | :----                                                                | :---                             |
| 0x72C-0x731 | Zombie kills for each counselor | Updated for current counselor when going into Change menu |
