
# Nichibutsu M68000 (Armed F) FPGA Implementation

FPGA compatible core of Nichibutsu M68000 (Terra Force Based) arcade hardware for [**MiSTerFPGA**](https://github.com/MiSTer-devel/Main_MiSTer/wiki) written by [**Darren Olafson**](https://twitter.com/Darren__O). Terra Force PCB (different board revisions) donated by [**@atrac17**](https://twitter.com/_atrac17) / [**@djhardrich**](https://twitter.com/djhardrich).

The intent is for this core to be a 1:1 implementation of the Nichibutsu (Terra Force based) 68000 hardware. Currently in an alpha state, this core is in active development with assistance from [**atrac17**](https://github.com/atrac17).

<br>
<img width="" height="" src="https://user-images.githubusercontent.com/32810066/170655863-5b1418f1-1020-46f6-9e72-6ad08ec96b49.png">
<br>

## Supported Games

| Title | Status | Released | Protection | Unprotected Sets |
|------|---------|----------|------------|------------------|
[**Terra Force**](https://en.wikipedia.org/wiki/Nihon_Bussan)           | **W.I.P**   | N         | NB1414M4 | **terrafb, terrafjb**   |
[**Kozure Ōkami**](https://en.wikipedia.org/wiki/Nihon_Bussan)          | **W.I.P**   | N         | NB1414M4 | N/A                     |
[**Chouji Meikyuu Legion**](https://en.wikipedia.org/wiki/Nihon_Bussan) | **W.I.P**   | N         | NB1414M4 | **legionjb, legionjb2** |
[**Crazy Climber 2**](https://en.wikipedia.org/wiki/Nihon_Bussan)       | **W.I.P**   | N         | NB1414M4 | N/A                     |
[**Armed F**](https://en.wikipedia.org/wiki/Formation_Armed_F)          | Implemented | **Y**     | None     | N/A                     |
[**Tatakae! Big Fighter**](https://en.wikipedia.org/wiki/Nihon_Bussan)  | Pending     | N         | i8751    | N/A                     |

## External Modules

|Name| Purpose | Author |
|----|---------|--------|
| [**fx68k**](https://github.com/ijor/fx68k)                                                   | [**Motorola 68000 CPU**](https://en.wikipedia.org/wiki/Motorola_68000) | Jorge Cwik                    |
| [**t80**](https://opencores.org/projects/t80)                                                | [**Zilog Z80 CPU**](https://en.wikipedia.org/wiki/Zilog_Z80)           | Daniel Wallner                |
| [**jtopl2**](https://github.com/jotego/jtopl)                                                | [**Yamaha OPL 2**](https://en.wikipedia.org/wiki/Yamaha_OPL#OPL2)      | Jose Tejada                   |
| [**jt8751**](https://github.com/jotego/jtframe/blob/master/hdl/cpu/jtframe_8751mcu.v)        | [**Intel 8751**](https://en.wikipedia.org/wiki/Intel_8051)             | Oregano Systems / Jose Tejada |

# Known Issues / Tasks

- ~~Clock domains need to be verified~~  
- H/V clock timings for CRT need to be verified  
- ~~Sprite Handling for 6MHz pxl clk and sdram memory controller~~  
- Screen Flip implementation  
- ~~Player 2 controls tied to screen flip/table mode~~  
- ~~Y/C video output~~  
- Dot Crawl on Y/C video output  
- ~~Implement keyboard handler / service menu~~  
- Reverse engineer Terra Force and provide schematics  
- Protection MCU i8571 implementation (**Tatakae! Big Fighter**)    
- Protection Chip **NB1414M4** implementation  

# PCB Check List

FPGA implementation is based on Terra Force and will be verified against an authentic Nichibutsu U.S.A Terra Force PCB and bootleg PCB.

Reverse engineering of an authentic Terra Force PCB will be done by [**Darren Olafson**](https://twitter.com/Darren__O) and schematics will be included in the repository.

### Clock Information

H-Sync      | V-Sync      | Source                                   |
------------|-------------|------------------------------------------|
15.625kHz   | 59.092993Hz | [DSLogic +](https://www.is.gd/AFLNoI)    |

### Crystal Oscillators

Location           | Freq (MHz) | Use          |
-------------------|------------|--------------|
C1 (Top Board)     | 16.000     | M68000       |
F1 (Bottom Board)  | 24.000     | Z80 / YM3812 |

**Pixel clock:** 6.00 MHz

**Estimated geometry:**

    387 pixels/line
  
    262 pixels/line

### Main Components

Location | Chip | Use |
---------|------|-----|
D1  (Top Board)      | [**Motorola 68000 CPU**](https://en.wikipedia.org/wiki/Motorola_68000)   | Main CPU                                  |
H17 (Bottom Board)   | [**Zilog Z80 CPU**](https://en.wikipedia.org/wiki/Zilog_Z80)             | Sound CPU                                 |
K15 (Bottom Board)   | [**Yamaha YM3812**](https://en.wikipedia.org/wiki/Yamaha_OPL#OPL2)       | OPL                                       |
D1  (Daughter Board) | [**Intel 8751**](https://en.wikipedia.org/wiki/Intel_8051)               | Protection MCU **(Tatakae! Big Fighter)** |

### Nichibutsu Custom Components

| Location | Chip | Use | PCB | ROM set |
|----------|-----|------|-----|---------|
A9  (Top Board)  | [**NB1414M4**](https://raw.githubusercontent.com/va7deo/TerraForce/main/doc/Terra%20Force%2026494/Terra%20Force%2026494%20Front.jpg?token=GHSAT0AAAAAABKJR6W6CVBT5FROK2UKF75KYTXT2BA) | Protection IC | <u>**Chouji Meikyuu Legion**</u> <br> <u>**Terra Force**</u> <br> <u>**Kozure Ōkami**</u> <br> <u>**Crazy Climber 2**</u> | legion, legionj, legionj2 <br> terraf, terrafu, terrafj <br> kozure <br> cclimbr2, cclimbr2a |

# Debugging Features

A hidden debug menu can be accessed for **Terra Force, Kozure Ōkami, Chouji Meikyuu Legion, Crazy Climber 2 and Armed F**.  For more information on each debugging menu per game visit "[**The Cutting Room Floor**](https://tcrf.net/Category:Games_developed_by_Nichibutsu)".<br><br>**Tatakae! Big Fighter** has debug options set via dipswitches.

**Follow these steps to access the debug menu:** 

- Turn on **Service Menu** in the debug OSD options or press **F2**. Then turn on **Debug Menu** or press and hold **R**, **A**, **S** . For **Crazy Climber 2** press and hold  **Up** and **R**.<br><br>
- Toggle the **Service Menu** option off; then toggle **Debug Menu** off in the OSD or keep those corresponding keys held and press **F2**.<br><br>
- To utilize the OSD method for accessing the Debug Method do the following, **Pause when OSD is open: Off**.
<br>

|Armed F Debugging Menu Example|
|:--:|
|<table> <tr><th>Character Graphics ROM</th><th>Object Graphics ROM</th><th>Vertical Scroll Test</th></tr><tr><td>![1](https://tcrf.net/images/4/46/FormationArmedFArcDebug1.png)</td><td> ![2](https://tcrf.net/images/9/9d/FormationArmedFArcDebug2.png)</td><td>![3](https://tcrf.net/images/a/a8/FormationArmedFArcDebug3.png)</td></tr></table> |

# Control Layout

Game | Joystick | Service Menu | Shared Controls | Dip Default |
:---: | :---: | :---: | :---: | :---: |
Armed F| 8-Way | <p align="center"><img width="120" height="160" src="https://user-images.githubusercontent.com/32810066/167619407-71b94a8b-2013-4166-bbc8-0de3c1e442d2.png"></p> | Upright | **Upright**

<br>

- Upright cabinets share a **1L3B** control panel layout. Players are required to switch controller. If set the cabinet type is set to table, the screen inverts for cocktail mode per player and has multiple controls. <br><br>Push button 3 may have no function in game, but corresponds to the original hardware and service menu.

<br>

### Keyboard Handler

- Keyboard inputs mapped to mame defaults for all functions.

|Services|Coin/Start|
|--|--|
|<table> <tr><th>Functions</th><th>Keymap</th></tr><tr><td>Test</td><td>F2</td></tr><tr><td>Reset</td><td>F3</td></tr><tr><td>Service</td><td>9</td></tr><tr><td>Pause</td><td>P</td></tr> </table> | <table><tr><th>Functions</th><th>Keymap</th><tr><tr><td>P1 Start</td><td>1</td></tr><tr><td>P2 Start</td><td>2</td></tr><tr><td>P1 Coin</td><td>5</td></tr><tr><td>P2 Coin</td><td>6</td></tr> </table>|

|Player 1|Player 2|
|--|--|
|<table> <tr><th>Functions</th><th>Keymap</th></tr><tr><td>P1 Up</td><td>Up</td></tr><tr><td>P1 Down</td><td>Down</td></tr><tr><td>P1 Left</td><td>Left</td></tr><tr><td>P1 Right</td><td>Right</td></tr><tr><td>P1 Bttn 1</td><td>L-CTRL</td></tr><tr><td>P1 Bttn 2</td><td>L-ALT</td></tr><tr><td>P1 Bttn 3</td><td>Space</td></tr> </table> | <table> <tr><th>Functions</th><th>Keymap</th></tr><tr><td>P2 Up</td><td>R</td></tr><tr><td>P2 Down</td><td>F</td></tr><tr><td>P2 Left</td><td>D</td></tr><tr><td>P2 Right</td><td>G</td></tr><tr><td>P2 Bttn 1</td><td>A</td></tr><tr><td>P2 Bttn 2</td><td>S</td></tr><tr><td>P2 Bttn 3</td><td>Q</td></tr> </table>|

# Support

Please consider showing support for this and future projects via [**Ko-fi**](https://ko-fi.com/darreno). While it isn't necessary, it's greatly appreciated.

# Licensing

Contact the author for special licensing needs. Otherwise follow the GPLv2 license attached.
