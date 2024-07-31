# GB300

The **Sup+ GB300** (short for Game Box) is a cheap handheld that emulates video game consoles. You can find it on AliExpress and other sites. The cheapest way to get it (around 9 dollars) is via AliExpress's "Pick 3 and Save" (aka "Bundle Deals") if it's available there (which isn't always the case) and you can find two more items from that page, like a bigger TF card (e.g. ALUNX brand), or simply buy three GB300. In general, the GB300 is a few dollars more than the cheapest game consoles on AliExpress often dubbed the Famiclones, but the GB300 offers eight classic consoles (instead of just the Famicom), comes with _way_ more games (even on the Famicom), you can add your own games, and you can save (states and sometimes standard GBA battery saves).

Some see it as a clone of the (usually a bit more expensive) Data Frog SF2000, which however is a bit different. Because the [SF2000 has already been documented](https://vonmillhausen.github.io/sf2000/), this page focusses primarily on the differences.

This document is work in progress but mostly finished now. Large parts target developers and anyone willing to mod the device, but the page has an [FAQ for Players](#faq-for-players) as well. Feel free to contact me, `numma_cway`, on Discord or Reddit. You can also create a fork and pull request, or open an issue on [Github](https://github.com/nummacway/gb300/). If you have any questions, join the `#data_frog_sf2000` channel on the [Retro Handhelds Discord](https://discord.gg/retrohandhelds) (choose SF2000 during onboarding). There is also a [`Gb300 dev` thread](https://discord.com/channels/741895796315914271/1195581037003165796) on that Discord, but that's for developers and not really for end users.


## Table of Contents

User topics:
- [FAQ for Players](#faq-for-players)
- [Tools](#tools)
    - [multicore](#multicore)

In-depth technical analysis:
- [Hardware](#hardware)
- [General Firmware Features](#general-firmware-features)
    - [Saving](#saving)
- [ROMs and Gameplay](#roms-and-gameplay)
    - [Nintendo Entertainment System](#nintendo-entertainment-system)
        - [Famicom Disk System](#famicom-disk-system)
        - [V.R. Technology VT02/VT03](#vr-technology-vt02vt03)
    - [PC Engine](#pc-engine)
    - [Super Nintendo Entertainment System](#super-nintendo-entertainment-system)
    - [SEGA Mega Drive, SEGA Master System and SEGA Game Gear](#sega-mega-drive-sega-master-system-and-sega-game-gear)
        - [SEGA Mega Drive](#sega-mega-drive)
        - [SEGA PICO](#sega-pico)
        - [SEGA Master System](#sega-master-system)
        - [SEGA Game Gear](#sega-game-gear)
        - [Other SEGA Consoles](#other-sega-consoles)
    - [Game Boy](#game-boy)
    - [Game Boy Color](#game-boy-color)
    - [Game Boy Advance](#game-boy-advance)

Developer and modding topics:
- [Resources](#resources)
    - [Fonts](#fonts)
    - [Images](#images)
    - [ROM Lists](#rom-lists)
    - [Foldername.ini](#foldernameini)
    - [KeyMapInfo.kmp](#keymapinfokmp)
      - [multicore Key Maps](#multicore-key-maps)
    - [Sounds](#sounds)
    - [Other Files](#other-files)

 Appendix:
- [List of ROMs](#list-of-roms)
    - [List of PCE Games](#list-of-pce-games)


## FAQ for Players

**tl;dr:**

***Don't's:***
- Do **not** use Tadpole (a tool made solely for the SF2000). Otherwise, you **will** break your GB300 (except for GBA).
- Do **not** use a quick charger (more than 5 volts). Otherwise, you **will** destroy your GB300.
- Do **not** insert the battery with reversed polarity. Otherwise, you **will** destroy your GB300.

***Do's:***
- Patch the bootloader (either with GB300 Tool or [manually](https://vonmillhausen.github.io/sf2000/#bootloader-bug)) **before** making **any** other changes to the TF card, including getting a new one. Otherwise, you **will** break your GB300 (sooner or later).
- Optional: Get [GB300 Tool](https://github.com/nummacway/gb300tool/releases/latest).
- Optional: Get [multicore](#multicore).
- Optional: Read this document.

| Topic | What you should know | What you can/should do |
| ----- | -------------------- | ---------------------- |
| **Device** | Giant step from _any_ cheaper handheld (the so-called Famiclones). 918&thinsp;MHz MIPS CPU with 128&thinsp;MB of high-latency DDR2. No networking whatsoever. | Buy one. Or spend a few dollars more on the Data Frog SF2000 which has the same performance but a better screen. |
| **Screen** | 2.8" 320×240px LCD screen. Viewing angle from the sides is extremely bad, up/down is alright. Very bright black in dark environments. Cannot adjust brightness. | It is possible to take the screen from a SF2000 or [buy this spare screen](https://www.aliexpress.com/item/1005006458765669.html). Screen swaps require a different firmware. |
| **Buttons** | D-pad isn't very accurate. | You might want to [add some tape](https://www.youtube.com/watch?v=sMuv5TzCedY#t=377) (requires opening the console). |
| **TV Out** | Comes with a 70&thinsp;cm cable from 3-pin 2.5&thinsp;mm audio to two RCA (cinch) jacks. Can be used with most older TVs. If you are in Europe, your TV might instead have SCART, for which there are adapters. Some TVs don't like the signal in general. | If possible, use NTSC to prevent unnecessary vertical scaling. Do not plug in the cable before the device has fully booted. |
| **Sound** | Mono speaker. No phone plug. | — |
| **Battery** | Standard 800&thinsp;mAh 18650-type battery. Play and charge time both are around 2 hours (power consumption: 1 W). Device has overcharge protection but not undercharge. Do not use a quick charger, especially not if turned on. Cannot be charged with a USB-C-to-USB-C cable. | Do not leave the device turned on or undercharge will kill the battery. You can change the battery to a better one as 800&thinsp;mAh really small. You can buy them online and in e-cigarettes stores. Mind the polarity when replacing the battery or you will destroy the device. |
| **TF Card<br>(=microSD)** | 8 GB card, 1.75 GiB free. Device is picky about the cards it takes at all, and cheap ones are more likely to work. Included card works with any standard TF reader or SD reader via any TF-SD adapter. You could use a phone to access the TF card, but that's not convenient. SDXC is supported (SDHC and SDXC hardware are exactly the same), but you will have to use FAT32 which is non-standard for SDXC for unknown reasons (FAT32 supports 16&thinsp;TiB, but SDXC is limited to 2&thinsp;TB). Yet some AliExpress microSDXC cards come preformatted to FAT32 so you can use those right away. We don't know the maximum TF capacity, but 64&thinsp;GB works. | Get a larger TF card and make sure it's formatted in FAT32 (Rufus has been suggested for SDXC, as Windows won't let you format SDXC in FAT32). Then just copy the content from your old card (you might want to copy the `bios` folder first, just in case). Before you copy any stuff to the new TF Card, patch the bootloader with your current card. The latter and general device management are greatly simplified by using [GB300 Tool](https://github.com/nummacway/gb300tool/releases/). |
| **Firmware<br>General** | Closed-source OS that uses `libretro` cores (see the [list of stock emulators](#roms-and-gameplay)). Also supports Sega Master System and Kids Computer Pico ROMs but doesn't come with ROMs for these. Could play FDS and VT02/03 ROMs, but these features are disabled. Some SNES and many GBA games are slow. Access pause menu by pressing Start+Select. | [Patch the bootloader](https://vonmillhausen.github.io/sf2000/#bootloader-bug) to spare yourself of a bug in its FAT32 implementation. Copy `gba_bios.bin` where the firmware expects it to slightly improve compatibility. ([GB300 Tool](https://github.com/nummacway/gb300tool/releases/) can do both for you. It can also enable FDS and VT02/03 support.) Install fan project [multicore](#multicore) on your TF card to add loads of new platforms and greatly improve GBA performance. It can be a bit tricky to add ROMs and configure it, but GB300 Tool can ease that once multicore is installed. |
| **ROMs** | Comes with 6267 ROMs. Expecially the NES ROMs are ofter modified (hacks) according to No-Intro. There are seven hardcoded lists that you can edit with GB300 Tool and a modded version of Frogtool. | Create the folder `ROMS` and put your own ROMs there. Also create a subfolder `save` there so you can save. You can [patch Game Gear ROMs into SMS ROMs](#sega-game-gear). |
| **Saving** | Stock GBA does not reliably battery-save (battery is the correct term for an in-game save), Pokémon mini (on multicore) can save as well, and all other emulators (including multicore) cannot battery-save _at all_. (Soft resets can load a battery if it was saved without leaving the emulator in the meantime, so you can complete Pokémon.) States work alright but you cannot (usually) use them in other emulators. | Use only save states. Import GBA battery saves (`.sav`) by placing them in both, `ROMS` and `GBA`, and Pokémon mini saves (`.eep`) in `ROMS\save`. |
| **Interface** | There are [55 image files](#images) and a [text file](#foldernameini) that you can modify. You can also modify [sounds](#sounds) or replace the [font file](#fonts). | Use [GB300 Tool](https://github.com/nummacway/gb300tool/releases/) to edit the image files since the file names and file format are both very odd. It can also edit the text file. Sounds can be converted using [Kerokero - SF2000 BGM Tool](https://github.com/Dteyn/SF2000_BGM_Tool). The font file is a standard TTF font. |
| **Button<br>Mapping** | You can reassign buttons for each console, but the editor is seriously bugged. | [GB300 Tool](https://github.com/nummacway/gb300tool/releases/) can tell you what you actually bind. |
| **Accessories** | Supports connecting a very rare type of wired gamepad for the second player. A likely source has been discovered only recently. Wireless gamepads are not supported. | It is believed that [this](https://www.aliexpress.com/item/1005006900177735.html) is the only way to buy one. [This listing](https://www.aliexpress.com/item/1005007161518444.html) has a bundle. Both don't ship worldwide. For the first listing, make sure to buy the wired version. Note that they sell them in pairs, but you can only connect one. |
| **Support** | There is no known way to contact the manufacturer because we don't know who that is. | Ask on [Retro Handhelds Discord](https://discord.gg/retrohandhelds) (choose SF2000 during onboarding) or [reddit](https://www.reddit.com/r/GB300/). |


## Tools

Most tools designed for the SF2000 don't work. Tools are often incompatible because not only is the BIOS different, but also the `Resources` have different names. This is especially true for Tadpole. Just starting it already patches your ROM lists and will break all default ROMs. It will only leave the GBA (because the files used for the GBA on the GB300 are used for the arcade on the SF2000, but there is no `ARCADE` folder for Tadpole to scan). If you did use Tadpole, look for the files in the Resources folder with the current date and restore the backups Tadpole put there.

The following tools were made specifically for the GB300:
* [multicore for GB300](https://github.com/tzubertowski/gb300_multicore/releases) by osaka, Prosty and the creators of the original multicore for SF2000.
* [GB300 Tool](https://github.com/nummacway/gb300tool/releases/latest) by me (numma_cway); now with multicore support! Includes tons of features, including the functionality of any non-sound-related tool below.
* [Customized _Frogtool_ (Beta)](https://discord.com/channels/741895796315914271/1195581037003165796/1211025634680119327) by tzlion (original version) and Dteyn (GB300 patch), used for rebuilding the console-dependent ROM lists.
* [GB300 Boot Logo Changer](https://dteyn.github.io/sf2000/tools/bootLogoChangerGB300.htm) by Dteyn

Tools for the SF2000 that should work for the GB300:
* [BIOS CRC-32 Patcher](https://vonmillhausen.github.io/sf2000/tools/biosCRC32Patcher.htm) by Von Millhausen
* [Generic Image Tool](https://vonmillhausen.github.io/sf2000/tools/genericImageTool.htm) by Von Millhausen, to convert to and from RGB565 and BGRA8888 images
* [Kerokero - SF2000 BGM Tool](https://github.com/Dteyn/SF2000_BGM_Tool) by Dteyn
* [Save State Tool](https://vonmillhausen.github.io/sf2000/tools/saveStateTool.htm) by Von Millhausen – Due to the strange CPU architecture, people don't think that converting save states makes any sense. But you can use it to extract screenshots. This tool relies on the first four bytes storing the compressed size. However, NES ROMs with an `.nfc` extension (that includes file names inside ZIP, obfuscated ZIP and thumbnailed files) and multicore don't have that, because compressed size is also in the last four bytes, meaning these don't work.
* [Silent menu music](https://vonmillhausen.github.io/sf2000/sounds/silentMusic/pagefile.sys) by Von Millhausen
* [Silent Sounds Pack](https://github.com/Dteyn/sf2000/raw/main/sounds/silentSounds/SF2000_Silent_Sounds_Pack.zip) by Dteyn

Other links:
* [Retro Handhelds Discord](https://discord.gg/retrohandhelds), select Data Frog SF2000 during onboarding and join the `#data_frog_sf2000` channel
  * [`Gb300 dev` thread](https://discord.com/channels/741895796315914271/1195581037003165796) 
  * [`GB300 screen swap` thread](https://discord.com/channels/741895796315914271/1197607372277940314)
* [SF2000 Community Compatibility list](https://docs.google.com/spreadsheets/d/19TCedWEKFXlnS2dlmLxk1BcnlHrX-MSVrKwEURuiU0E/edit#gid=1327539659)


### multicore

Discord users osaka (`bnister`) and Prosty (`_prosty`) brought multicore to GB300 on April 27th, 2024. This means that you can now access many more emulators and enjoy way better GBA performance.

#### Installing and using multicore with GB300 Tool

The easiest way to use multicore is probably via [GB300 Tool](https://github.com/nummacway/gb300tool/releases/latest). Download the ZIP file (the one with the cube icon) and start the tool. Enter your TF reader's drive letter and follow these steps:

1. Check the first checkbox on the BIOS/Device page that comes up right after entering your drive letter.
2. Put the TF card in your GB300 and boot. It will display some progress indicator during boot for a few seconds.
3. Turn off the GB300 and put the card in your TF reader again.
4. Put `bios` and `cores` folders from the [7-Zip file](https://github.com/tzubertowski/gb300_multicore/releases/) on your existing TF card (so the `bios` folder overwrites (merges with) the existing folder).
5. Restart GB300 Tool to make it notice that you now have multicore.
6. Select one of the first eight tabs in GB300 Tool and either click "Add..." or drop your ROMs on the tool. It will ask you for the multicore core, but will have recommendations for you. If GB300 Tool tells you that you need a BIOS, you need to put it the `bios` folder on your TF card. Here's a [list of cores](https://docs.google.com/spreadsheets/d/1BDPqLwRcY2cN7tObuyW7RzLw8oGyY9XGLS1D4jLgz2Q/edit#gid=1430267016). Most cores link to libretro's docs with more information on BIOS.

You only have to do steps 1 to 5 once.


#### Installing and using multicore manually

1. Before you do anything else: [Patch the bootloader](https://vonmillhausen.github.io/sf2000/#bootloader-bug). Really! Spare yourself the possible trouble with the device not booting because of a buggy FAT-32 implementation.
2. Put the [7-Zip file](https://github.com/tzubertowski/gb300_multicore/releases/)'s content on your existing TF card.
3. For each "core" (the term means emulator – the GB300 CPU is single-core) you want, create a subfolder with its name in `ROMS` and put your ROMs for that core in its subfolder. Here's a [list of cores](https://docs.google.com/spreadsheets/d/1BDPqLwRcY2cN7tObuyW7RzLw8oGyY9XGLS1D4jLgz2Q/edit#gid=1430267016).
4. Run `make-romlist` found in the root directory of your TF card now. It does not actually make a ROM _list_ but creates so-called stubs. These are zero-byte (empty) `.gba` files passed to the GBA emulator. However, the GBA emulator was given a hook that will run multicore if the file name conforms to a certain file name pattern.
   * If you don't want to run the script, you can create the stubs yourself. The pattern is `CORENAME;FILENAME.gba`. Example: `Zero Wing.MD` is placed in `ROMS\sega` to be launched with the `sega` core. Then you need to create `ROMS\sega;Zero Wing.MD.gba`, `ROMS\sega;Zero Wing.MD.agb` or `ROMS\sega;Zero Wing.MD.gbz`.
5. Many of the emulators added by _multicore_ require one or more BIOS files. In the Google Spreadsheet linked above, there is one link to libretro docs per core. That linked page will explain what BIOS files you need (the section is missing if an emulator does not use BIOS files). BIOS files must be placed in the `bios` folder of your TF card.

Note: Multicore saves in `ROMS\save`. The thumbnail (screenshot) is named and formatted like always, but with no payload other than the image, as the state is in another file that isn't compressed.

Making thumbnailed multicore files is super weird: The _filename_ (without the extension) of the `.zfc`, `.zsf`, `.zpc`, `.zmd` or `.zgb` file must conform to the multicore pattern, however, the _extension_ is pulled from the contained file. So the file name inside the ZIP file does not matter, but must end on `.gba`, `.zgb` or `.agb`. Basically you could take any stock GBA file, and for example name it `sega;Zero Wing.md.zsf` to make it launch `ROMS\sega\Zero Wing.md` with the `sega` core.


## Hardware

The hardware is very similar to the SF2000. The processor is the same 918 MHz MIPS processor (HiChip/HCSEMI B210, overclocked from 810 MHz) with 128 MB of high-latency DDR2 RAM, originally designed to be used in DVD players and set-top boxes. The most important difference is the vertical form factor which makes the GB300 look a bit like the (much heavier) Game Boy Color. The GB300 lacks the SF2000's "digital analog stick" and the buttons feel somewhat cheap.

The screen is a cheap LCD screen compared to the SF2000’s IPS screen. The horizontal viewing angle (sideways) is extremely small, but vertical is alright. Especially when playing dark games in a dark room, the very bright black is an issue, as neither device has a brightness control. People who love the GB300 for its form factor, working sound volume control and straight-forward interface have bought an SF2000 just to [swap its screen into the GB300](https://discord.com/channels/741895796315914271/1197607372277940314), so the rest of the device can't be that bad, hmm? You can [buy a spare screen](https://www.aliexpress.com/item/1005006458765669.html), too. The GB300's default screen has diagonal(!) screen tearing. It isn't really noticeable unless there's flashing or fading.

Because it lacks the arcade support accounting for 2.75 GB on the SF2000, the device ships with only a 8 GB TF/microSDHC card (42 MB of which aren't allocated to a partition), formatted FAT32. It includes the firmware and the default set of 6267 ROMs. This leaves around 1.75 GB for your own ROMs. Actually, there's more space if you follow the manual: All the ROMs are just for demonstration and you are supposed to delete them right when you receive the console, even though the menus are hardcoded to exactly these files. The GB300 is picky in terms of which TF cards it will accept.

The device comes with a 70&thinsp;cm (28") cable from a 2.5mm male audio plug to two male RCA (cinch) plugs. The yellow RCA plug is for composite video and the red one for sound. You can plug them into older TVs either directly or via a SCART adapter. If you plug the cable in the GB300, its own screen will be turned off. The TV output has a better resolution (640x480) than the internal screen's 320x240. If your TV doesn't care, use NTSC 480i to avoid unnecessary vertical scaling to 576i. NTSC outputs a vertically pixel-perfect result of the user interface. Unlike the SF2000, the TV signal will be fine while charging the GB300. Do not plug in the AV cable until the device has completely booted (that includes not plugging in the cable before switching the device on, meaning that the full-size bootlogo is never used).

The GB300 works with the _wired_ gamepads that sometimes ship with some other cheap(er) consoles. You cannot normally buy them individually and the GB300 wasn't sold bundled with them either until [this listing](https://www.aliexpress.com/item/1005007161518444.html) appeared in mid/late June 2024. These devices work for solely the second player in games that support that. _Wireless_ gamepads don't work on the GB300, e.g. the gamepad bundled with the SF900 TV stick that works with the SF2000. Note that neither of these complies with industry standards like USB or BT, so they don't have any use with computers, laptops or mainstream consoles. If your gamepad connects to any of these, it's definitely not compatible with the GB300. There are two types of the wired gamepads for consoles like the GB300, the common 5-wire used by all the cheaper Famiclones and the super-rare 4-wire. The GB300 only supports the latter. A [source for these](https://www.aliexpress.com/item/1005006900177735.html) has been discovered in early June 2024. This comes despite the fact that "external gamepad double against" is even promoted on the front of the GB300's box...

The GB300 is powered by a standard 18650 battery that you can easily change. The default battery appears to have overcharge protection (the charging current will drop when the battery is full), yet the green charging light will not turn off. If the battery is very low (crashes and glitches), it will take a little under 4&thinsp;VAh until it stops charging. This suggests that the capacity is lower than the SF2000's 1750 mAh. This is supported by the manual and box listing 800 mAh, and people reporting that the (light pink and completely unlabeled) battery of the GB300 is lighter than the SF2000's. More recently, people have reported receiving labelled batteries, confirming the 800 mAh. Neither device has undercharge protection, so leaving the device on with a low battery can kill the battery. One person reported that their GB300 came with the power switch in the 'ON' position and therefore a dead battery. Buying a new battery worked. If you buy a new battery, consider one with both, over- and undercharge protection. Although the SF2000 takes flat batteries, the GB300 seems to require some manipulation to its contact springs due to the console's case design. Mind the polarity when replacing the battery, or you will destroy the console. The GB300 initially charges with around 2.5 to 2.9&thinsp;W, which decreases as it charges.

Another similar device is the 8-Bit King, but that's an HDMI stick with wireless gamepads. It's usually around one dollar cheaper than the GB300 and lacks support for SNES, GBA and MD/SMS because it has less/worse RAM. There is a [hack](https://discord.com/channels/741895796315914271/1165850204713537637/1208756612051771413) for limited MD/SMS support though. The 8-Bit King too plays your own ROMs and can save.


## General Firmware Features

The GB300's stock firmware emulates the following devices:
* Nintendo Entertainment System (Famicom)
* Famicom Disk System (disabled, but can be enabled)
* V.R. Technology VT02/VT03 (disabled, but can be enabled)
* PC Engine (Turbografx-16)
* Super Nintendo Entertainment System (Super Famicom)
* SEGA Master System (SEGA Mark III)
* SEGA Mega Drive (SEGA Genesis)
* SEGA PICO (Kids Computer Pico) – debatable if this is different from the Mega Drive
* Game Boy
* Game Boy Color
* Game Boy Advance

Compared to the SF2000 stock firmware, the GB300 lacks the arcade section and adds the PCE. Both platforms can be added using multicore. While multicore's Mednafen PCE Fast works well, multicore has MAME 2000 instead of Final Burn Alpha (a MAME fork), because multicore team couldn't get FBA or newer versions of MAME to run properly. Performance and compatibility with larger-filesize games are both worse on multicore MAME 2000 than on SF2000's FBA, e.g. `m2k` can only load 34 of the 228 ROMs that ship with the SF2000, and Wildfang is the only one of these that runs at full speed. The ROMs that ship with the SF2000 are generally more graphically challenging. Two rules of thumb: You can espect multicore's MAME 2000 to have issues with ROMs that are bigger than around 1 MB (compressed) or look better than your usual Mega Drive games. MAME fails to load ROMs larger than around 25 MB compressed or 60 MB uncompressed due to a lack of available memory.

There is no chance you could enable FDS or VTxx on the SF2000 (FDS can be enabled in multicore). If you don't mind the weird colors, you could also play Game Gear games that do not make use of the Start button. Change the `.gg` extension to `.sms` to make them show up. smspower.org has color patches ("GG2SMS") for around 200 GG games (they list 185 different games, for which there are 225 versions on No-Intro, but not all versions are supported). There's a [list of GG2SMS patches that work on the GB300](#sega-game-gear) in this document.

There's actually two things called "firmware" on the GB300: There is a small bootloader (512KiB) that loads the firmware from the TF card. You should [patch that bootloader](https://vonmillhausen.github.io/sf2000/#bootloader-bug) to prevent issues when tampering with files in the `BIOS` folder on the TF card. Really. This patch works for the GB300 as well and takes only a few seconds. With the bootloader separated from the rest of the firmware and the firmware on a TF card, any modding attempts are relatively safe.

The SF2000 firmware does not work on the GB300. There is no known way to retrieve an updated official firmware because the manufacturer is unknown. See [multicore](#multicore) below for a modified firmware for the GB300 made by the users. A [port of GB300's firmware to SF2000](https://vonmillhausen.github.io/sf2000/#gb300-firmware-ported) has been made so people can enjoy the easier interface and use GB300 Tool. There is also a [version with multicore](https://discord.com/channels/741895796315914271/1092831839955193987/1237247978520055828). The default BIOS dates to the 15th of December, 2023. There might be an older version dating back to the 26th of October, 2023, but the only evidence of that one was corrupted.


### Saving

***Savestates:*** The device features four save _states_ per game which allow saving at any point (press Start+Select). However, they are usually incompatible between different emulators. If you want to try anyway, you first need to extract them from their `zlib`-based format (same as on the SF2000). There is [a tool for that](https://vonmillhausen.github.io/sf2000/tools/saveStateTool.htm). `.nfc` ROMs (including those in compressed files) use uncompressed save states which are not supported by that tool. Tests with VBA-M's GBA save states (after extracting the `gzip` file that is VBA-M's save state format) didn't work (black screen on the GB300).

***Battery Saves:*** Normally, you would be able to exchange _battery_ files between emulators. These are the files that store the savegames created by the _games'_ save feature (should one exist). However, there's an issue with them on the GB300:
* GBA: If you want to _load_ a battery file from another emulator, place it in the `ROMS` folder (not the `save` subfolder). For stock ROMs, is sometimes uses the (user) `ROMS` folder and sometimes the `GBA` folder, so put your saves in both folders. Saving is a bit more complicated. Sometimes it works, sometimes it doesn't. And even if you can load a battery before turning off the device, this does not guarantee that you will still be able to load it after turning off the device. Saving and loading without switching off the GB300 in the meantime works for GBA games. Should you need to get your battery file from the device, load your state and save in-game. Repeat until you can load your battery after restarting the GB300. Then you should have a working battery file.
* GB/GBC: Many people are concerned about Pokémon games which force a soft reset after beating the Champ. Luckily, this is no issue at all for the stock GB/GBC emulator, if you saved in-game after you last loaded a state (or started a new game). This will normally be the case if you make sure that you _do not save a state before you're back in the game_. In other words: Do not save and load a state during the intro. How can you quickly test this with any GB/GBC/GBA emulator in the world? Save a game, press A+B+Start+Select and you should be able to load the battery. This is not an emulator feature but implemented by nearly all ROMs.
* Need to do more research on the other emulators.

Multicore generally cannot battery-save if the core conforms to `libretro`'s standards. However, `pokem` for example does not conform to libretro's standards, so it can battery-save (`.eep`). It can also create/load states (`.state0` to `.state3`) from the PC version of PokeMini. Note that _all_ `pokem` games create a `.eep`, even those without a save feature like _Zany Cards_.


## ROMs and Gameplay

___* * * This entire section is about the stock firmware. It does not apply to [multicore](#multicore) which cannot run zipped ROMs and therefore none of the stock ROMs either. Multicore does not take away any functionality, so installing multicore will still allow you to use the stock emulators with stock ROMs and any custom ROMs (unless the filename contains a semicolon and ends on any GBA extension). * * *___

To play your own games, create the folder `ROMS` (case-insensitive like all filenames on FAT-32) on the TF card and put your ROMs there. You can also use single-file ZIP files to save memory. Also create a `save` subfolder (`ROMS\save`), because the GB300 will not create one for you and fail saving if that subfolder is missing.

Stock ROMs however come in their own format. The first `0xEA00` bytes are a 144x208 pixel RGB565 image with no header whatsoever. (You can technically change the size of the image, see the `Foldername.ini` section of this document.) After that comes a ZIP file, obfuscated with five differences to keep you from opening it:
* The magic numbers are different as they bear the initials of [CubeTac](https://bootleggames.fandom.com/wiki/Cube_Technology)'s **W**ang **Q**un**W**ei, or Wise Wang. There are three magic numbers in a standard ZIP file with one contained file:
  * The local header magic number is `0x57515703` instead of the standard `0x504B0304`.
  * The central directory header magic number is `0x57515702` instead of the standard `0x504B0102`.
  * The end of central directory header magic number is `0x57515701` instead of the standard `0x504B0506`.
* Both occurences of the file name in these headers had their bytes `xor 0xE5`'d.

Changing the things above will give you a standard ZIP file. At least 7-Zip is already fine if you just fix the local header's magic number, even though the file inside will have a strange filename then. The obfuscation is purely optional, but found in all stock ROMs.

| Emulator              | Version   | Git Commit | File Extensions                                                            | Bitmask      |
| --------------------- | --------- | ---------- | -------------------------------------------------------------------------- | ------------ |
| _XZip-XUnZip_         | _unknown_ | _unknown_  | `.bkp`, `.zip`                                                             | `0x00000100` |
| _(thumbnailed file)_  |           |            | `.zfc`, `.zsf`, `.zpc`, `.zmd`, `.zgb`                                     | `0x00000300` |
| _XZip-XUnZip_         | _unknown_ | _unknown_  | `.zfb`*                                                                    | `0x00000300` |
| **Snes9x 2005**       | v1.36     | _unknown_  | `.smc`, `.fig`, `.sfc`, `.gd3`, `.gd7`, `.dx2`, `.bsx`, `.swc`             | `0x08000000` |
| **FCEUmm**            | _none_    | `7cdfc7e`  | `.nes`, `.fds`, `.unf`, ~~`.unif`~~                                        | `0x01000000` |
| **wiseemu/libvrt**    | _unknown_ | _unknown_  | `.nfc`                                                                     | `0x02000000` |
| **TGB Dual**          | v0.8.3    | `9be31d3`  | `.gbc`, `.gb`, `.sgb`                                                      | `0x20000000` |
| **gpSP**              | v0.91     | `261b2db`  | `.gba`, ~~`.bin`~~, `.agb`, `.gbz`                                         | `0x10000000` |
| **PicoDrive**         | 1.91      | `cbc93b6`  | `.bin`, `.md`, `.smd`, `.gen`, ~~`.32x`~~, ~~`.cue`~~, ~~`.iso`~~, `.sms`  | `0x04000000` |
| **Mednafen PCE Fast** | v0.9.38.7 | _unknown_  | `.pce`, ~~`.cue`~~, ~~`.ccd`~~, ~~`.chd`~~                                 | `0x80000000` |

*&#32;= `.zfb` does have the bitmask `0x00000300` used for thumbnailed files, but the GB300 can only show thumbnails for the third to seventh file extension in its list (which is identical to the one above). `.zfb` is the eighth in that internal list so there is no thumbnail. The lack of a thumbnail for `.zfb` is funny because on the SF2000, the sole use of `.zfb` is to provide the thumbnail for a file in another directory. ZFB is short for ZIP Final Burn (an arcade emulator), despite not containing zipped content on the SF2000. See [above](#installing-and-using-multicore-manually) for more information on creating a multicore thumbnail.

_wiseemu_ or _libvrt_ is the name of this emulator on platforms where it is a seperate file. It was created by Wise Wang. The the other emulators are from `libretro`. If they were used in that context, they'd report all the given extensions to `libretro`, but the GB300 does not display the stroke-out ones. `.bin` files are associated with PicoDrive, not gpSP, so they are stroke-out for the latter.

ZIP and thumbnailed files are both allowed to be optionally obfuscated. And yes, even a `.zip` file is allowed to be obfuscated.

The bitmask is located in the BIOS where it comes _after_ the extension. The block with this data is close to the end of the BIOS file. Open it a hex editor and search for `NFC` because that string does not occur anywhere else. `.nfc` is associated with a different NES emulator (wiseemu) than the `.fds`, `.nes` and `.unf` (FCEUmm). The `.nfc` extension is seen in 280 of the 868 stock ROMs. The most notable difference is that wiseemu's save states are uncompressed. Loading `.fds` fails for both, but can be [enabled](#famicom-disk-system).

The GB300 relies on the extension (or, more precisely, the extension's bitmask) to decide what to do with the file:
* Display a thumbnail?
* Search for ZIP or WQW header and decompress the file before starting the ROM? All files with a thumbnail also belong into this category. This means that there is no way to have a thumbnail without compression.
* Choose the emulator (see the table above).

There are no signs of other supported emulators, but it looks like MPEG-2 support is included but inaccessible. If you force the GB300 to display `.chd` files, opening one will cause it to load indefinitely, even for the tiniest PCE-CD game out there, _Hawaiian Island Girls_ (under 3 megabytes). Same goes for `.cue` files no matter if MD-CD or PCE-CD.


### Nintendo Entertainment System

Compared to the SF2000, the following games are missing:
* `Island.zfc`
* `Jump Jump.zfc`
* `Little Witch.zfc`
* `Mad Xmas.zfc`
* `Metroid.zfc`
* `Miner1.zfc`*
* `Nut Cracky.zfc`
* `Pobble.zfc`
* `Sodoku.zfc`*

Games with an asterisk are duplicates of games that are still on the device.

The GB300 comes with two NES emulators: _FCEUmm_ is associated with `.nes`, `.fds`, `.unf`, whereas a mysterious other emulator called _wiseemu_ is used for `.nfc`. To find out which one is used for which stock ROM, see [this list](https://vonmillhausen.github.io/sf2000/defaultRoms/defaultRomsNoIntroCheck.htm). FCEUmm seems to be the better one for NES. You can see the difference in Galaxian which clearly glitches/tears.


#### Famicom Disk System

To enable FDS support in stock, do any of the following:
- Open `bios\bisrv.asd` in a hex editor and stuff `0xa7f00b` at offsets `0x34f170`, `0x34f194` and `0x34f1b0`. Remember that you need to [rehash the BIOS](https://vonmillhausen.github.io/sf2000/tools/biosCRC32Patcher.htm) after making changes to it.
- Enable the feature in the most recent version of GB300 Tool.

In both cases, you must put `disksys.rom` in `ROMS` (does not respect your Foldername.ini). Now you can simply start `.fds` images like any other ROM.

To play double-sided disks, you need a way to eject them, turn them around and insert them. Virtually, that is. This is done by binding keys to `0x0A00` (FDS Turn Disk) and `0x0B00` (FDS Eject/Insert). This is not possible by the GB300's GUI but you can use GB300 Tool or a hex editor. More details are found [here](#keymapinfokmp). To be clear: You must press three buttons to turn the disk: Eject, Turn and Eject again.

Thanks to osaka (`bnister`) for finding all this out.


#### V.R. Technology VT02/VT03

Now we get to something the SF2000 cannot do, not even with multicore: V.R. Technology made some Famicom clones (Famiclones) that weren't just clones but technically more advanced than the Famicom, called the [VTxx](https://bootleggames.fandom.com/wiki/VTxx). As Sup+ was mostly known for making Famiclones (400-in-1, 500-in-1, a.s.o.) before making the GB300, the GB300 retains Wise Wang's strange Famiclone emulator. Discord user `bnister` (osaka) did some research on this. I won't explain the steps required to enable it, as you can simply check the two checkboxes in GB300 Tool's July 2024 version (the one that reads v1.0-final, not the one that reads v1.0). Files must have the `.nfc` extension.

You can get VTxx ROMs from _Project Plug-and-Play_ and from the Internet Archive. Note that the tagging of ROMs in Project Plug-and-Play is inconsistent and their games are more prone to issues than those from the Internet Archive. Notes on Project Plug-and-Play:
* Most games that are untagged or tagged VT02 are actually NES games. You can play them in FCEUmm and wiseemu without changing the iNES header.
* Some VT03 games are not actually tagged as such, e.g. most/all TimeTop games are actually VT03.
* Many VT03-tagged games in Project Plug-and-Play do not load on the GB300 or the sprites are glitched. Games available for different VTxx chips and games from Jungletac rarely load. Not a single original CubeTac game works. TimeTop's games and a few of CubeTac's hacks (e.g. Arrow Maze) however work very well.
* Many soccer-related VT03 games do load but do not register input.
* A few untagged VT03 slot machine games from Jungletac do load, but have weird colors.
* Some games changed platform during development. The first demo of Street Dance works on FCEUmm, whereas the second demo and the final bundle with Hit-Mouse require VTxx emulation. Said game is pointless on the GB300's nameless emulator since PCM audio does not work. (And you cannot connect your dance mat to the GB300, making this game even more pointless.)
* Roughly half of the VT09-tagged games in Project Plug-and-Play do load, but these have weird interlaced graphics, making most of them unplayable. The other half does not load.

Only about 10% of the OneBus games in Project Plug-and-Play work. OneBus is a requirement for VTxx. Checking for OneBus is easy in Project Plug-and-Play files because you just look at address `0x05` in the iNES header. If it's `0x00`, the game has no CHR ROM and is therefore a OneBus ROM.

This is the full list of the 784 OneBus games from Project Plug-and-Play (2023-12-31) that you can play on the GB300:

| Group (`.7z`) | Game (`.nes`)                    | Type |
| ----- | ---------------- | ---- |
| **Cube Tech\Hacks**
| Arkanoid | Pocket | VT03 |
| Balloon Fight |Beat Ballute | VT03 |
| Battle City | S Move | VT03 |
| Brush Roller | Pengoo | VT03 |
| Duck Maze | Maze Trooper | VT03 |
| Elevator Action | Spy vs. Spy Combat | VT03 |
| Flappy | Pro Genius | VT03 |
| International Cricket | Cricket World Cup 2003 | VT02 |
| Magmax | 3D Machine | VT03 |
| Penguin-kun Wars | Fire Ball | VT03 |
| Pooyan | Maze Arrow (VT03) | VT03 |
| Raid on Bungeling Bay | Aero Gyrodine | VT03 |
| Spelunker | Ghost Zero Trap | VT03 |
| Sqoon | Deep Fighter | VT03 |
| Thexder | Xtreme Robot | VT03 |
| **Hummer**
| Ping Pong | Ping Pong | VT02 |
| **Inventor**
| Street Dance | Street Dance (rev1)* | VT02 |
| **Jungletac**
| Bubble Blaster | Bubble Blaster (rev0) | VT02 |
| Go Bang | Go Bang | VT02 |
| Number Quest | Number Quest | VT02 |
| Pool Pro | Billiards Master | VT02 |
| Submarine War | Submarine War (VT09) | VT02 |
| **Nice Code Software**
| Mad Xmas | Lucky Time (VT03) | VT03 |
| **Nice Code Software\Intellivision**
| Snafu | Star (VT03) | VT03 |
| **TimeTop**
| Adventure | Adventure | VT03 |
| Bomb Boy | Bomb Boy | VT03 |
| Firebolt | Firebolt | VT03 |
| Gun-Force | Gun-Force* | VT03 |
| Risker | Risker* | VT03 |
| Stone Age | Stone Age* | VT03 |
| **TimeTop\Hacks**
| F-1 Race | Crazy Speed* | VT03 |
| **Unknown Developer**
| Duck & Dodge | Duck & Dodge | VT03 |
| Hip-Hop Scotch | Hip-Hop Scotch | VT03 |
| Snowstorm | Snowstorm | VT03 |
| **Unknown Developer\Hacks**
| Pinball | Radium Star | VT03 |

The type given above is what _I_ think is the cartridge type (VT02: requires mapper 12; VT03 also requires LUT patch to not look green-ish). NintendulatorNRS distributed by Project Plug-and-Play does not always agree with me. Games with an asterisk have some glitches that do not technically prevent you from playing them.

UM6578, VT32, VT168 and VT369 never load.

Research is still going on. I am also preparing a list of VT03 ROMs from the Internet Archive that you can play on the GB300.


### PC Engine

Performance of PCE is really good. Even the "3D" racing games run very smoothly.

There seems to be no way to get it to run TurboGrafx-CD games.

None of the only five SuperGrafx games work. The screen is all black or purple, and that's not even deterministic. _Aldynes_ has music for some seconds, but then crashes. Pressing the start button before it crashes gives you a purple screen with some glitches at the bottom center, then the game instantly freezes. None of the games will freeze the GB300.


### Super Nintendo Entertainment System

With the arcade removed, the SNES and GBA are the only two systems where games can have a bad performance. However, the SNES has a lot less issues than the GBA.

Final Knockout does work on the GB300. It was broken on the SF2000 because the bytes from `0xA8000` to `0xBFFFF` of the .zsf file were replaced with unidentified data.

Compared to the SF2000, the following file is missing:
* `手柄测试.zsf`


### SEGA Mega Drive, SEGA Master System and SEGA Game Gear

#### SEGA Mega Drive

Compared to the SF2000, the following game is missing:
* `007 Shitou - The Duel.zmd`

But one thing got better: Instead of the 225 broken thumbnails on the SF2000, there are only 45 on the GB300. These thumbnails were saved in BGRA8888 instead of RGB565. However, the dimension is correct. Despite the thumbnail taking up twice the space, the device is still able to find the archive and run the game. The following MD games have no working thumbnail on the GB300:
* `AWS Pro Moves Soccer.zmd`
* `Coach K College Basketball.zmd`
* `Davis Cup II.zmd`
* `Davis Cup Tennis.zmd`
* `Double Clutch.zmd`
* `ESPN Baseball Tonight.zmd`
* `Fun 'n Games.zmd`
* `HardBall III.zmd`
* `James Pond III.zmd`
* `King of the Monsters.zmd`
* `Madden NFL '94.zmd`
* `Madden NFL 95.zmd`
* `Madden NFL 96.zmd`
* `Mario Lemieux Hockey.zmd`
* `Mr. Nutz Hoppin' Mad.zmd`
* `Muhammad Ali Heavyweight Boxing.zmd`
* `NBA Live 95.zmd`
* `NFL '95.zmd`
* `NHL 96.zmd`
* `NHL 97.zmd`
* `NHL 98.zmd`
* `PGA Tour 96.zmd`
* `PGA Tour Golf III.zmd`
* `PGA Tour Golf.zmd`
* `Pro Quarterback.zmd`
* `Puyo Puyo 2.zmd`
* `R.B.I. Baseball '94.zmd`
* `Ren & Stimpy Show Presents - Stimpy's Invention.zmd`
* `RoboCop versus The Terminator.zmd`
* `Rolo to the Rescue.zmd`
* `Sampras Tennis 96.zmd`
* `Scooby-Doo Mystery.zmd`
* `Spirou.zmd`
* `Sub-Terrania.zmd`
* `Super Baseball 2020.zmd`
* `The Smurfs Travel the World.zmd`
* `Tintin au Tibet.zmd`
* `Triple Play - Gold Edition.zmd`
* `Triple Play 96.zmd`
* `Wolverine - Adamantium Rage.zmd`
* `World Series Baseball '95.zmd`
* `World Series Baseball '96.zmd`
* `World Series Baseball 98.zmd`
* `World Series Baseball.zmd`
* `Wu Kong Wai Zhuan.zmd`

GB300 Tool can fix them for you.


#### SEGA PICO

The SEGA PICO is technically identical to the Mega Drive. It looks like a children's laptop, but you place the book-shaped cartridges ("storyware") where you would expect a screen and connect it to a TV for video. As this is simply the Mega Drive, the GB300 can run SEGA PICO ~~games~~ software. No PICO storybook ROMs ship with the console though.

Even current versions of PicoDrive do not support its successor, the SEGA Advanced Pico Beena.


#### SEGA Master System

Only MD is advertised and there are no SMS games included. The device will still play them if you add them yourself. The button assignments are strange, though:
* SMS Button 1 is called B by the GB300 and defaults to B.
* SMS Button 2 is called C by the GB300 and defaults to R.
* The pause key that is located on the SMS physical console (not on the gamepad) is on Start.


#### SEGA Game Gear

Most Game Gear games will load. (The games that don't load will have a black screen.) The Game Gear's resolution is 160x144, but the emulator displays this in the center of the SMS's 256x192 pixel viewport. Graphics outside the center 160x144 area sometimes make sense (so you basically have an extended vision) but in some cases they're glitched. Colors are severely glitched all the time, but Audio is fine. If you don't mind the weird colors, you could play, if it wasn't about one other thing: The Game Gear has a D-Pad (which corresponds to the GB300's D-Pad), a "1" button (which the GB300 calls B and is mapped to the B button by default) and a "2" button (which the GB300 calls C and is mapped to R by default). And then, well, there's the "Start" button, which isn't mapped at all and none of the key IDs I tried (-1 through 19) corresponded to it. The majority of the Game Gear games require you to press it to get past the title screen (or, in some cases, to start the game after picking options), most notably almost all Sega games (the only exception that I know of is the beta version of the _Sega Game Pack 4 in 1_). Of the 25 best-rated Game Gear games on MobyGames, there are only seven that aren't blunt ports from other GB300 consoles and only one of the latter does not require the use of the Start button: _Magical Puzzle: Popils_.

smspower.org has [patches for many games](https://www.smspower.org/Hacks/GameGearToMasterSystem), especially the unique ones, that will change them to SMS. The Start button seems to be an issue for them as well, because the SMS has no direct equivalent.
* The logical equivalent would be the Pause key, but it's located on the console and not on the gamepad. Most patches seem to be made for owners of the actual console, sitting a few meters away from it, so the Pause button is out of reach. A few patches use it anyway. This equals Start on your GB300.
* A few games do not make use of both of the gamepad's buttons, so hackers bind Start (and pause) to the unused one.
* You can connect an MD gamepad to the SMS, so hacks can make use of its dedicated Start button. A few hacks do that. Even after playing almost 500 GG2SMS hacks, I'm still not sure if this works on the GB300.
* A few hacks address the button issue on the title screen only, as both available buttons have no use there anyway, but leave the pause function non-working. That's not an issue for you, as the GB300 has its own pause menu. A very few _games_ pause themselves when you enter the GB300's pause menu, even though you had no Start button and you cannot resume anymore...
* All of the above options will work on the GB300, but by far the most common Start replacement in these patches is moving the second controller's D-pad down. Sadly, that's the most problematic one.

Here's a complete list of Game Gear to SMS conversions that you can play on your GB300 because they work and do not require an external gamepad. There are 88 different games (103 including regional versions, revisions and re-releases). The following tables uses A and B for the buttons to make them easier to distinguish.

| No-Intro Name <small style="font-weight:normal;">(Other Names)</small> | ID | CRC-32<br>Before | Best<br>Patch | CRC-32<br>After | Pause | Start<br>Does | Outside<br>160×144 |
| ---- | -- | ------ | ----- | ----- | ----- | ---------- | ------- |
| **Aa Harimanada (Japan)** | [0002](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0002) | `1d17d2a0` | [v0.6](https://www.smspower.org/Hacks/AaHarimanada-GG-GG2SMS) | `442398bb` | ? | - | useful |
| **Arcade Classics (USA)** | [0013](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0013) | `3deca813` | [v1.0](https://www.smspower.org/Hacks/ArcadeClassics-GG-GG2SMS) | `3ea9582d` | ? | - | blank |
| **Arena (USA, Europe)** <small>(Arena Maze of Death)</small> | [0015](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0015) | `7cb3facf` | [v0.31](https://www.smspower.org/Hacks/ArenaMazeOfDeath-GG-GG2SMS) | `f9a9e13f` | ? | - | glitched |
| **Ariel - The Little Mermaid (USA, Europe, Brazil) (En)** | [0016](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0016) | `97e3a18c` | [v1.1](https://www.smspower.org/Hacks/ArielTheLittleMermaid-GG-GG2SMS) | `98c81182` | ? | - | useful |
| **Baku Baku (USA)** | [0025](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0025) | `8d8bfdc4` | [v1.1](https://www.smspower.org/Hacks/BakuBakuAnimal-GG-GG2SMS) | `2360c031` | Start | Start | blank |
| **Batman Forever (World)** | [0028](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0028) | `618b19e2` | [v1.1](https://www.smspower.org/Hacks/BatmanForever-GG-GG2SMS) | `5c80865b` | Start | Start | glitched |
| **Batter Up (USA)<br>Gear Stadium (Japan)** | [0030](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0030)<br>[0128](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0128) | `16448209`<br>`0e300223` | [v0.3](https://www.smspower.org/Hacks/BatterUp-GG-GG2SMS) | `df35fa20`<br>`148b2704` | ? | - | glitched |
| **Battleship - The Classic Naval Combat Game (USA)** | [0031](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0031) | `e9987511` | [v1.1](https://www.smspower.org/Hacks/Battleship-GG-GG2SMS) | `500eff47` | ? | Start | glitched |
| **Battletoads (Japan, Europe) (En)<br>Battletoads (USA)** | [0032](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0032)<br>[0033](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0033) | `cb3cd075`<br>`817cc0ca` | [v1.1](https://www.smspower.org/Hacks/Battletoads-GG-GG2SMS) | `7aa39825`<br>(for both) | Start | Start | glitched |
| **Berlin no Kabe (Japan)** | [0037](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0037) | `325b1797` | [v1.2](https://www.smspower.org/Hacks/BerlinNoKabe-GG-GG2SMS) | `e8b0a245` | Start | Start | blank |
| **Bishoujo Senshi Sailor Moon S (Japan)** | [0038](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0038) | `fe7374d2` | [v0.2](https://www.smspower.org/Hacks/BishoujoSenshiSailorMoonS-GG-GG2SMS) | `95f50b0e` | Start | Start | glitched |
| **Bugs Bunny in Double Trouble (USA, Europe)** | [0042](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0042) | `5c34d9cd` | [v0.1](https://www.smspower.org/Hacks/BugsBunnyInDoubleTrouble-GG-GG2SMS) | `4542f03d` | ? | Restart | glitched |
| **Bust-A-Move (USA)** | [0043](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0043) | `c90f29ef` | [v1.0](https://www.smspower.org/Hacks/PuzzleBobble-GG-GG2SMS) | `920970f1` | B | Restart | background |
| **Buster Ball (Japan)** | [0044](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0044) | `7cb079d0` | [v1.2](https://www.smspower.org/Hacks/BusterBall-GG-GG2SMS) | `3bf5c4d8` | Start | Start | blank |
| **Buster Fight (Japan) (En,Ja)** | [0045](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0045) | `a72a1753` | [v1.0](https://www.smspower.org/Hacks/BusterFight-GG-GG2SMS) | `f76c577c` | ? | Freeze | useful |
| **Captain America and the Avengers (USA)** | [0047](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0047) | `5675dfdd` | [v0.4](https://www.smspower.org/Hacks/CaptainAmerica-GG-GG2SMS) | `c8c650f9` | Start | Start | glitched |
| **Chessmaster, The (USA, Europe, Brazil) (En)** | [0053](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0053) | `da811ba6` | [v1.1](https://www.smspower.org/Hacks/Chessmaster-GG-GG2SMS) | `27696636` | Start | Start | blank |
| **Clutch Hitter (USA)** | [0060](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0060) | `d228a467` | [v1.1](https://www.smspower.org/Hacks/ClutchHitter-GG-GG2SMS) | `f042b9c7` | Start, A | Start | blank |
| **Crazy Faces (Europe) (Proto)** | [0493](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0493) | `46ad6257` | [v1.0](https://www.smspower.org/Hacks/CrazyFaces-GG-GG2SMS) | `28e07c39` | Start | Start | blank |
| **Crystal Warriors (USA, Europe)** | [0067](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0067) | `529c864e` | [v1.11](https://www.smspower.org/Hacks/CrystalWarriors-GG-GG2SMS) | `702483aa` | Start | Start | glitched |
| **Dr. Franken (Europe) (Demo)** | [0485](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0485) | `c9907dce` | [v1.0](https://www.smspower.org/Hacks/DrFranken-GG-GG2SMS) | `810adea6` | ? | Crash | useful |
| **Ganbare Gorby! (Japan)** <small>(Factory Panic, Crazy Company)</small> | [0125](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0125) | `a1f2f4a1` | [v0.4](https://www.smspower.org/Hacks/FactoryPanic-GG-GG2SMS) | `ac0e19ea` | A | - | glitched |
| **Fatal Fury Special (USA)<br>Fatal Fury Special (Europe)<br>Garou Densetsu Special (Japan)** | [0108](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0108)<br>[0458](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0458)<br>[0127](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0127) | `449787e2`<br>`fbd76387`<br>`9afb6f33`  | [v1.2](https://www.smspower.org/Hacks/FatalFurySpecial-GG-GG2SMS) | `4b7f7b2d`<br>`1c6cfac7`<br> | Start | Start | glitched |
| **Frogger (USA) (Proto)** | [0117](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0117) | `02bbf994` | [v0.92a](https://www.smspower.org/Hacks/Frogger-GG-GG2SMS) | `71112e1e` | Start | Start | glitched |
| **Galaga '91 (Japan)** <small>(Galaga 2)</small> | [0122](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0122) | `0593ba24` | [v1.0](https://www.smspower.org/Hacks/Galaga91-GG-GG2SMS) | `812d56ee` | B | - | mostly blank |
| **GG Aleste (Japan) (En)<br>GG Aleste (Japan) (En) (Aleste Collection)** | [0132](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0132)<br>[0807](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0807) | `1b80a75b`<br>`0a49407d` | [v1.2](https://www.smspower.org/Hacks/GGAleste-GG-GG2SMS) | `c84576ba`<br>`d98c919c` | B | - | background |
| **Power Strike II (Japan, Europe) (En)** <small>(GG Aleste II)</small> | [0286](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0286) | `09de1528` | [v1.6](https://www.smspower.org/Hacks/GGAlesteII-GG-GG2SMS) | `f0b7cef6` | Start | Start | useful |
| **GG Portrait - Pai Chen (Japan)** | [0134](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0134) | `695cc120` | [v1.0](https://www.smspower.org/Hacks/GGPortraitPaiChen-GG-GG2SMS) | `11875dea` | ? | - | glitched |
| **GG Portrait - Yuuki Akira (Japan)** | [0135](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0135) | `51159f8f` | [v1.0](https://www.smspower.org/Hacks/GGPortraitYuukiAkira-GG-GG2SMS) | `ec9023ee` | ? | - | blank |
| **GG Shinobi, The (Japan)<br>Shinobi (World) (Rev A)** | [0138](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0138)<br>[0137](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0137) | `83926bd1`<br>`30f1c984` | [v1.1](https://www.smspower.org/Hacks/GGShinobi-GG-GG2SMS) | `c79d7b37`<br>`85b0c578` | B | - | background |
| **GP Rider (World)** | [0141](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0141) | `876e9b72` | [v1.0](https://www.smspower.org/Hacks/GPRider-GG-GG2SMS) | `48966551` | B | - | glitched |
| **Griffin (Japan)** | [0143](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0143) | `a93e8b0f` | [v1.01](https://www.smspower.org/Hacks/Griffin-GG-GG2SMS) | `8653cb4d` | ? | - | background |
| **Gunstar Heroes (Japan)** | [0144](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0144) | `c3c52767` | [v0.9](https://www.smspower.org/Hacks/GunstarHeroes-GG-GG2SMS) | `9fbd6261` | Start | Start | background |
| **Tarot no Yakata (Japan)** <small>(House of Tarot)</small> | [0152](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0152) | `57834c03` | [v1.0](https://www.smspower.org/Hacks/HouseOfTarot-GG-GG2SMS) | `fe6180fd` | ? | Restart | glitched |
| **Itchy & Scratchy Game, The (USA, Europe)** | [0162](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0162) | `44e7e2da` | [v0.99](https://www.smspower.org/Hacks/ItchyAndScratchyGame-GG-GG2SMS) | `f15d8e09` | ? | - | useful |
| **J.League GG Pro-Striker '94 (Japan)** | [0163](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0163) | `a12a28a0` | [v1.0](https://www.smspower.org/Hacks/JLeagueGGProStriker94-GG-GG2SMS) | `628518c7` | ? | - | background |
| **J.League Soccer - Dream Eleven (Japan)** | [0164](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0164) | `abddf0eb` | [v1.0](https://www.smspower.org/Hacks/JLeagueSoccerDreamEleven-GG-GG2SMS) | `79b83f96` | ? | - | background |
| **James Pond 3 - Operation Starfi5h (Europe)** | [0166](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0166) | `68bb7f71` | [v1.1](https://www.smspower.org/Hacks/JamesPond3-GG-GG2SMS) | `71bb012e` | ? | - | background |
| **Jeopardy! (USA)** | [0168](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0168) | `d7820c21` | [v1.0](https://www.smspower.org/Hacks/Jeopardy-GG-GG2SMS) | `7066f925` | ? | Freeze | glitched |
| **Jeopardy! - Sports Edition (USA)** | [0169](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0169) | `2dd850b7` | [v1.0](https://www.smspower.org/Hacks/JeopardySportsEdition-GG-GG2SMS) | `fe274916` | ? | Restart | glitched |
| **Junction (Japan) (En)** | [0174](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0174) | `a8ef36a7` | [v1.1](https://www.smspower.org/Hacks/Junction-GG-GG2SMS) | `203f4abe` | B | - | blank |
| **Kinetic Connection (Japan)** | [0183](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0183) | `4af7f2aa` | [v1.0](https://www.smspower.org/Hacks/KineticConnection-GG-GG2SMS) | `ae1e11b4` | A | - | blank |
| **Madden NFL 95 (USA)** | [0200](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0200) | `75c71ebf` | [0.1](https://www.smspower.org/Hacks/MaddenNFL95-GG-GG2SMS) | `dbc73e55` | ? | - | glitched |
| **Magical Puzzle Popils (World) (En,Ja)** | [0209](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0209) | `cf6d7bc5` | [v1.1](https://www.smspower.org/Hacks/MagicalPuzzlePopils-GG-GG2SMS) | `e48ac8a3` | ? | - | blank |
| **Mighty Morphin Power Rangers (USA, Europe)** | [0224](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0224) | `9289dfcc` | [v0.6](https://www.smspower.org/Hacks/MightyMorphinPowerRangers-GG-GG2SMS) | `e6c9e6c3` | Start | Start | glitched |
| **Mighty Morphin Power Rangers - The Movie (USA, Europe, Brazil) (En)** | [0225](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0225) | `b47c19e5` | [v0.61](https://www.smspower.org/Hacks/MightyMorphinPowerRangersTheMovie-GG-GG2SMS) | `66e2943f` | Start | Start | glitched |
| **Nazo Puyo 2 (Japan)** | [0236](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0236) | `73939de4` | [v0.9](https://www.smspower.org/Hacks/NazoPuyo2-GG-GG2SMS) | `a0715293` | ? | - | blank |
| **NFL Quarterback Club 96 (USA)** | [0246](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0246) | `c348e53a` | [v1.0](https://www.smspower.org/Hacks/NFLQuarterbackClub96-GG-GG2SMS) | `e130321e` | ? | Restart | useful |
| **Ninja Gaiden (USA, Europe, Brazil) (En)<br>Ninja Gaiden (Japan)** | [0251](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0251)<br>[0250](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0250) | `c578756b`<br>`20ef017a` | [v1.2](https://www.smspower.org/Hacks/NinjaGaiden-GG-GG2SMS) | `244bc257`<br>(for both) | ? | - | glitched |
| **Pac-Attack (USA)** | [0261](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0261) | `9273ee2c` | [v1.0](https://www.smspower.org/Hacks/PacAttack-GG-GG2SMS) | `c57cd3f1` | B | - | blank |
| **Pac-in-Time (USA) (Proto)** | [0262](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0262) | `64c28e20` | [v1.01](https://www.smspower.org/Hacks/PacInTime-GG-GG2SMS) | `5c547a12` | ? | - | glitched |
| **Pac-Man (Japan) (En)** | [0263](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0263) | `a16c5e58` | [v0.91](https://www.smspower.org/Hacks/PacMan-GG-GG2SMS) | `aaeb0ebd` | A | - | glitched |
| **Paperboy 2 (USA)** | [0266](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0266) | `8b2c454b` | [v1.0](https://www.smspower.org/Hacks/Paperboy2-GG-GG2SMS) | `0d491508` | ? | - | glitched |
| **Pengo (Europe)<br>Pengo (Japan)** | [0268](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0268)<br>[0267](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0267) | `0da23cc1`<br>`ce863dba` | [v1.0<sub>nv</sub>](https://www.smspower.org/Hacks/Pengo-GG-GG2SMS) | `c466c41f`<br>`3bb9e266` | A | - | blank |
| **Phantasy Star Adventure (Japan)** | [0275](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0275) | `1a51579d` | [v1.1](https://www.smspower.org/Hacks/PhantasyStarAdventure-GG-GG2SMS) | `9a34f904` | ? | - | blank |
| **Pinball Dreams (USA)** | [0278](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0278) | `635c483a` | [v1.0](https://www.smspower.org/Hacks/PinballDreams-GG-GG2SMS) | `9b1e554a` | ? | - | glitched |
| **Pop Breaker (Japan)** | [0284](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0284) | `71deba5a` | [v1.2](https://www.smspower.org/Hacks/PopBreaker-GG-GG2SMS) | `70f930e7` | A+B, Start | Start | useful |
| **Primal Rage (USA, Europe)** | [0288](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0288) | `2a34b5c7` | [v1.0](https://www.smspower.org/Hacks/PrimalRage-GG-GG2SMS) | `4405645e` | ? | - | glitched |
| **Psychic World (USA, Europe, Brazil) (En) (Rev 1)** | [0295](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0295) | `73779b22` | [v1.0](https://www.smspower.org/Hacks/PsychicWorld-GG-GG2SMS) | `5da0dabd` | Start | Start | glitched |
| **Puyo Puyo (Japan) (En,Ja)** <small>(Puzlow Kids)</small> | [0298](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0298) | `d173a06f` | [v1.0](https://www.smspower.org/Hacks/PuyoPuyo-GG-GG2SMS) | `718376c0` | ? | - | blank |
| **Ristar (World)** <small>(Ristar - The Shooting Star)</small> | [0311](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0311) | `efe65b3b` | [v1.5](https://www.smspower.org/Hacks/RistarTheShootingStar-GG-GG2SMS) | `40707acd` | Start | Start | useful |
| **Royal Stone - Hirakareshi Toki no Tobira (Japan)** | [0315](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0315) | `445d7cd2`<br>`11d36c92` | [v1.2<sup>en</sup><br>v1.2<sup>jp</sup>](https://www.smspower.org/Hacks/RoyalStone-GG-GG2SMS) | `8e95363c` | ? | - | glitched |
| **Shanghai II (Japan) (Rev A)<br>Shanghai II (Japan)** | [0456](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0456)<br>[0325](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0325) | `81314249`<br>`2ae8c75f` | [v1.0](https://www.smspower.org/Hacks/ShanghaiII-GG-GG2SMS) | `a3852ecb`<br>(for both) | ? | - | glitched |
| **Shaq Fu (USA)** | [0326](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0326) | `6fcb8ab0` | [v0.1](https://www.smspower.org/Hacks/ShaqFu-GG-GG2SMS) | `52531898` | ? | - | glitched |
| **Shikinjou (Japan)** | [0327](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0327) | `9c5c7f53` | [v1.0](https://www.smspower.org/Hacks/Shikinjo-GG-GG2SMS) | `f670d70d` | B | Restart | blank |
| **Slider (USA, Europe)<br>Skweek (Japan)** | [0336](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0336)<br>[0335](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0335) | `4dc6f555`<br>`3d9c92c7` | [v1.2](https://www.smspower.org/Hacks/Slider-GG-GG2SMS) | `94a1f113`<br>`59ed0b75` | B | Restart | glitched |
| **Solitaire FunPak (USA)** | [0338](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0338) | `f6f24b75` | [v1.0](https://www.smspower.org/Hacks/SolitaireFunPak-GG-GG2SMS) | `c5e6fbb1` | N/A | - | blank |
| **Solitaire Poker (USA, Europe)<br>Ryuukyuu (Japan)** | [0339](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0339)<br>[0316](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0316) | `06f2fc46`<br>`95efd52b` | [v1.0](https://www.smspower.org/Hacks/SolitairePoker-GG-GG2SMS) | `08d4d240`<br>`74c64832` | ? | - | useful glitch |
| **Sonic Drift (Japan) (En)** | [0344](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0344) | `68f0a776` | [v0.4](https://www.smspower.org/Hacks/SonicDrift-GG-GG2SMS) | `43258667` | Start | Start | useful |
| **Sonic Drift 2 (World)** | [0346](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0346) | `d6e8a305` | [v1.1](https://www.smspower.org/Hacks/SonicDrift2-GG-GG2SMS) | `051fb276` | Start | Start | useful |
| **Sonic Labyrinth (World)<br>Sonic Labyrinth (USA, Europe) (Virtual Console)** | [0347](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0347)<br>[0802](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0802) | `5550173b` | [v1.1a](https://www.smspower.org/Hacks/SonicLabyrinth-GG-GG2SMS) | `1d51f6d3`<br>`fd304176` | A | - | glitched |
| **Sonic The Hedgehog - Triple Trouble (USA, Europe, Brazil) (En)** | [0352](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0352) | `d23a2a93` | [v0.4](https://www.smspower.org/Hacks/SonicTripleTrouble-GG-GG2SMS) | `1e40e1ea` | Start | Start | mostly useful |
| **Soukoban (Japan)** | [0354](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0354) | `0f3e3840` | [v1.1](https://www.smspower.org/Hacks/Soukoban-GG-GG2SMS) | `e35a6edd` | A | - | blank |
| **Spider-Man - X-Men - Arcade's Revenge (USA)** | [0357](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0357) | `742a372b` | [v1.1](https://www.smspower.org/Hacks/SpiderManXMenArcadesRevenge-GG-GG2SMS) | `35a0a649` | Start | Start | mostly useful |
| **Spirou (Europe) (Proto)** | [0359](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0359)<br>[0366](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0366) | `ab622adc` | [v1.1](https://www.smspower.org/Hacks/Spirou-GG-GG2SMS) | `a9dd9a7c` | ? | Restart | glitched |
| **Star Wars (USA, Europe)<br>Star Wars (USA, Europe) (Alt)** | [0365](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0365) | `db9bc599`<br>`0228769c` | [v0.2a](https://www.smspower.org/Hacks/StarWars-GG-GG2SMS) | `b7c53d7e`<br> | ? | - | useful ingame |
| **Streets of Rage (World)** <small>(Bare Knuckle)</small> | [0368](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0368) | `3d8bcf1d` | [v0.56](https://www.smspower.org/Hacks/StreetsOfRage-GG-GG2SMS) | `206d16e8` | Start | Start | glitched |
| **Streets of Rage 2 (World)** <small>(Bare Knuckle II)</small> | [0369](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0369) | `0b618409` | [v0.71](https://www.smspower.org/Hacks/StreetsOfRageII-GG-GG2SMS) | `45b8adb8` | Start | Start | glitched |
| **Super Golf (Japan)** | [0373](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0373) | `528cbbce` | [v0.1](https://www.smspower.org/Hacks/SuperGolf-GG-GG2SMS) | `6753bb03` | ? | - | glitched |
| **Super Momotarou Dentetsu III (Japan)** | [0375](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0375) | `b731bb80` | [v1.0](https://www.smspower.org/Hacks/SuperMomotarouDentetsuIII-GG-GG2SMS) | `65cbd81f` | N/A | - | blank |
| **Surf Ninjas (USA, Europe, Brazil) (En)** | [0383](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0383) | `284482a8` | [v1.0](https://www.smspower.org/Hacks/SurfNinjas-GG-GG2SMS) | `11d9e074` | ? | - | glitched |
| **Tails Adventure (World) (En,Ja)** <small>(Tails Adventures)</small><br>**~~Tails Adventure (World) (En,Ja) (Virtual Console)~~** | [0386](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0386)<br>[0803](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0803) | `5bb6e5d6`<br>`a354ea49` | [v1.5<br>v1.3](https://www.smspower.org/Hacks/TailsAdventures-GG-GG2SMS) | `8635c737`<br>`2935ba70` | Start | Start | useful |
| **Tarzan - Lord of the Jungle (Europe)** | [0394](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0394) | `ef3afe8b` | [v1.0](https://www.smspower.org/Hacks/Tarzan-GG-GG2SMS) | `52a3501b` | ? | - | glitched |
| **Tempo Jr. (World)** | [0398](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0398) | `de466796` | [v1.0](https://www.smspower.org/Hacks/TempoJr-GG-GG2SMS) | `4a707dfb` | Start | Start | glitched |
| **Tesserae (USA)** | [0402](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0402) | `bf696f94` | [v1.0](https://www.smspower.org/Hacks/Tesserae-GG-GG2SMS) | `415e61d0` | ? | - | useful glitch |
| **WildSnake (USA) (Proto)** | [0501](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0501) | `d460cc7f` | [v1.0](https://www.smspower.org/Hacks/WildSnake-GG-GG2SMS) | `0c45632b` | A | - | blank |
| **Yu Yu Hakusho II - Gekitou! Nanakyou no Tatakai (Japan)** | [0439](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0439) | `46ae9159` | [v1.0](https://www.smspower.org/Hacks/YuYuHakushoII-GG-GG2SMS) | `7afa5fa3` | A | - | useful glitch |
| **Yu Yu Hakusho - Horobishimono no Gyakushuu (Japan)** | [0438](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0438) | `88ebbf9e` | [v0.1](https://www.smspower.org/Hacks/YuYuHakusho-GG-GG2SMS) | `475b35f8` | ? | - | glitched |

Notes:
- The versioning scheme is inconsistent on SMS Power. This list uses v1.0 for releases called v1.

- I applied 387 different patches to 225 games and then played the resulting 490 ROMs. Should there be new hacks released after April 25th, they might be better than the ones above.
- The column "Pause" names the button to pause the game. "N/A" means that it is not necessary to pause the game according to SMS Power and probably not possible in the original game either. "?" means that you cannot pause with the GB300 alone. Usually, this means that a second gamepad's down button would work.
- The column "Start Does" tells you what the Start button does with the ROM: "Start" means that it does what the Start button is supposed to do. Dash means that nothing happens at all when you press Start. "Restart", "Freeze" and "Crash" all mean that you cannot play anymore after pressing Start. "Crash" means that the game glitches and instantly freezes.
- "Outside 160×144" explains what you can see outside the GG viewport: "blank" means nothing (solid color, usually black) and is preferable on games that do not scroll. "background" means that you can see the background but sprites still pop up only when entering the GG viewport. "glitched" describes that the non-GG part of the SMS viewport does not make sense and contains glibberish or random map tiles. "useful glitch" is used for games with a map that is only slightly bigger than the GG viewport. The map extends beyond the GG viewport, but can wrap to the other edge of the screen. "useful" means that the hacks use (almost) the entire SMS viewport and feel like a true SMS game.
- Notes on individual games:
  - _Aa Harimanada_: Version v0.5 fixed the colors after the end of a fight, but also introduced frequent health bar glitches. Since v0.6 makes use of the full screen, I consider v0.6 to be the best patch. If you're annoyed by the health bar glitch, use patch v0.4.
  - _Arcade Classics_: _Missile Command SEGA Version_ seems glitched.
  - _Berenstain Bears' Camping Adventure, The_: I did start a _Cave Adventure_ during my first test by pressing the Start button, but never managed to do that ever again. So this game is not listed.
  - _Frogger_: You'll often find a 256KiB ROM, but that's an overdump. The actual size is 128KiB and has the CRC-32 values above. The overdump has `da724710` and applying the patch results in a working ROM (`68b958f0`). The first half of the 256 KiB ROMs is identical to the 128 KiB ROM.
  - _Magical Puzzle Popils_: The map editor is completely glitched and you cannot exit it.
  - _Mighty Morphin Power Rangers - The Movie_: There is some white text on white background.
  - _Pinball Dreams_: Heavily glitched colors in transitions.
  - _Pop Breaker_: A+B is not only Pause but also self-destruct (I think that's only if you press them a bit longer). Beware!
  - _Royal Stone_: One of the two patches includes an English translation. There are many of these hacks that include a translation for the respective game, but this is the only one that works.
  - _Sega Game Pack 4 in 1_: Requires a second gamepad for the Start button and therefore isn't listed. The beta version didn't require the use of the Start button, but the hack results in a non-working ROM. I did test the earliest and latest beta version of all games whenever the final version required pressing the Start button and beta versions where available, but _Sega Game Pack 4 in 1_ was the only game whose beta version was different in this regard.
  - _Slider_/_Skweek_ and _Solitaire Poker_/_Ryuukyuu_: Use the respective version's patch. The wrong one does not work. This is in contrast to _Pengo_, where BcnAbel76's patch only works for the wrong ROM (the A button does not work if the patch is applied to the intended ROM), but nextvolume's patch is better and region-independent.
  - _Surf Ninjas_: Menu is completely glitched, but in-game it's okay.
  - _Tails Adventure_: v1.4 and v1.5 do not work for the Virtual Console one. Patch the normal version if you can.

There's a few more GG games that you can play, because technically they are normal SMS games and will therefore not have glitched colors. Some ROM sites even list these with an `.sms` extension although they're in the Game Gear category. Still, some ROMs will not work.
 
| No-Intro Name | ID | CRC-32 | Remarks |
| ------------- | -- | ------ | ------- |
| **Castle of Illusion Starring Mickey Mouse (USA, Europe, Brazil) (En)** | [0051](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0051) | `59840fd6` | Start button is working |
| **Cave Dude (USA) (Proto)** | [0503](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0503) | `cc521975` | Start button neither working nor mandatory |
| **Chase H.Q. (USA)** | [0480](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0480) | `c8381def` | Start button neither working nor mandatory |
| **Excellent Dizzy Collection, The (Europe)** | [0100](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0100) | `aa140c9c` | Game does not load |
| **Fantastic Dizzy (USA, Europe) (En,Fr,De,Es,It)** | [0105](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0105) | `c888222b` | No useful video, glitched audio |
| **Jang Pung II (Korea) (En) (Unl)** | [0469](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0469) | `76c5bdfb` | No useful video, no audio |
| **Mickey Mouse no Castle Illusion (Japan)** | [0050](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0050) | `9942b69b` | Start button is working |
| **Olympic Gold (Europe) (En,Fr,De,Es,It,Nl,Pt,Sv)** | [0256](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0256) | `1d93246e` | Start button is working |
| **Olympic Gold (Japan, USA, Brazil) (En,Fr,De,Es,It,Nl,Pt,Sv)** | [0257](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0257) | `a2f9c7af` | Start button is working |
| **OutRun Europa (USA)** | [0260](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0260) | `f037ec00` | Start button neither working nor mandatory |
| **Predator 2 (USA, Europe)** | [0287](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0287) | `e5f789b9` | Start button is working |
| **Prince of Persia (USA, Europe) (Beta)** | [0290](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0290) | `45f058d6` | Start button is working |
| **Prince of Persia (USA, Europe)** | [0289](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0289) | `311d2863` | Start button is working |
| **R.C. Grand Prix (USA)** | [0305](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0305) | `56201996` | Start button neither working nor mandatory |
| **Rastan Saga (Japan)** | [0306](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0306) | `9c76fb3a` | Start button is working |
| **Street Battle (USA) (Proto) (Unl)** | [0506](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0506) | `01a2d595` | No useful video, no audio |
| **Street Hero (USA) (Proto 1)** | [0489](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0489) | `9fa727a0` | No video, glitched audio |
| **Street Hero (USA) (Proto 2)** | [0490](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0490) | `fb481971` | Start button neither working nor mandatory |
| **Super Kick Off (Europe) (En,Fr,De,Es,It,Nl,Pt,Sv)** | [0374](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0374) | `10dbbef4` | Start button is working |
| **Taito Chase H.Q. (Japan)** | [0391](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0391) | `7bb81e3d` | Start button is working |
| **WWF Wrestlemania - Steel Cage Challenge (USA, Europe)** | [0433](https://datomatic.no-intro.org/index.php?page=show_record&s=25&n=0433) | `da8e95a9` | Start button neither working nor mandatory |

This list contains only ROMs listed on No-Intro. GoodGG has more ROM than No-Intro.

#### Other SEGA Consoles

Despite the undocumented support for PICO, SMS and options to make GG games work, you aren't that lucky with the other Sega consoles. If you change the GB300's BIOS to display them or change the extension (it doesn't matter which of these you do, with the exception of a very few SG-1000 games), the following things will happen:
* Sega CD games (`.bin` or `.cue`) will load indefinitely/freeze the console, even the tiniest and/or single-image ones (Ishii Hisaichi no Daiseikai, around 10 megabytes – the largest GBA games are 32 megabytes).
* Most Sega 32X games will not display anything at all, even if you put the correct BIOS in the root or Roms directory. A few games will display "something":
  * Both versions of _Knuckles' Chaotix_ display an error message.
  * _NBA Jam: Tournament Edition_, _Star Trek: Starfleet Academy - Starship Bridge Simulator_ and _WWF Raw_ display glibberish.
  * _Mars Check Program Version 1.0 (SDK Build) (Set 2)_ and _Toughman Contest_ display a green screen.
  * _Mars Check Program Version 1.0 (SDK Build) (Set 1)_ and _WWF WrestleMania: The Arcade Game_ display a red/orange screen.
* Most SG-1000 games will "load". (The games that don't load will freeze the device, e.g. Champion Baseball 40kB.) There is no video (black screen), but audio is fine. Buttons are also fine. (Note that applications like Home Basic likely don't have sound, so you can't tell if they're loading or not.) [SG to GG/SMS conversions](https://www.smspower.org/forums/post103058#103058) do _not_ work. Well, unless your goal is to analyse the screen tearing – _all_ these patches cause the entire screen to quickly change color. So: Do not try this if you're photosensitive.
* Advanced Pico Beena ROMs display a black screen.


### Game Boy

Compared to the SF2000, the following games are missing:
* `Aladdin [a].zgb`
* `Dokuhon Yume Goyoshin - Tenjin Kaisen 2.zgb`
* `Gakken Kanyouku Kotowaza 210.zgb`
* `Gluecksrad.zgb`
* `Goukaku Boy Series - Chuugaku Eijukugo 350.zgb`
* `Goukaku Boy Series - Eijukugo Target 1000.zgb`
* `Goukaku Boy Series - Eitango Target 1900.zgb`
* `Goukaku Boy Series - Gakuken Yojijukugo 288.zgb`
* `Goukaku Boy Series - Koukou Nyuushi Rika Anki Point 250.zgb`
* `Goukaku Boy Series - Nihonshi B Yougo Mondaishuu.zgb`
* `Goukaku Boy Series - Nihonshi Target 201.zgb`
* `Goukaku Boy Series - Rekishi 512.zgb`
* `Goukaku Boy Series - Sekaishi B Yougo Mondaishuu.zgb`
* `Goukaku Boy Series - Z Kai Chuugaku Eigo 1132.zgb`
* `Kirby's Pinball Land.zgb`
* `Teenage Mutant Ninja Turtles - Back from the Sewers [a].zgb`
* `Uchiiwai - Kyoudai Kami Waza no Puzzle Game.zgb`
* `X.zgb`


### Game Boy Color

Compared to the SF2000, the following games are missing:
* `Honkaku Taisen Shogi - Ayumu.zgb`
* `Konami GB Collection Vol.1.zgb`
* `Konami GB Collection Vol.2.zgb`
* `Konami GB Collection Vol.3.zgb`
* `Konami GB Collection Vol.4.zgb`
* `Pokemon - Gelbe Edition.zgb`
* `Pokemon - Gold Version.zgb`
* `Pokemon - Goldene Edition.zgb`
* `Pokemon - Kristall Edition.zgb`
* `Pokemon - Silberne Edition.zgb`
* `Pokemon - Silver Version.zgb`
* `Pokemon - Version Argent.zgb`
* `Pokemon - Version Cristal.zgb`
* `Pokemon - Version Jaune.zgb`
* `Pokemon - Version Or.zgb`
* `Pokemon - Versione Argento.zgb`
* `Pokemon - Versione Cristallo.zgb`
* `Pokemon - Versione Gialla.zgb`
* `Pokemon - Versione Oro.zgb`
* `Pokemon Card GB 2 - Team Great Rocket Visit.zgb`
* `Pokemon Crystal.zgb`
* `Shadowgate Return.zgb`
* `Shogi Oh.zgb`
* `Shougi 2.zgb`


### Game Boy Advance

Unlike all other consoles in the GB300, the ROM list for GBA is identical to the SF2000. Pokemon Glazed is the only non-MD file with an incorrect thumbnail, but not because of an incorrect format but because it's too big (346x500). It will still run.

The GB300 ships with the official (pirated) `gba_bios.bin` in the `bios` folder. This is, however, not the folder where the emulator will look for it. To use the official BIOS, copy it to `\GBA\mnt\sda1\bios\gba_bios.bin` and `\Roms\mnt\sda1\bios\gba_bios.bin` (create all of these folders if they do not exist). Thanks to `bnister` (osaka) for finding this out. One game that requires this procedure is _The Legend of Zelda - The Minish Cap_ (for the main menu), which however does not ship with the device. There are still games that don't work even with that BIOS. The BIOS does not seem to affect the performance. States with and without the BIOS are mutually incompatible. Loading a non-BIOS state when BIOS is active displays the GBA's boot animation and then starts the game. The battery from the state will not be present either.

As in the SF2000, performance varies heavily between games. And even language versions: Probably the oddest example here are the two Advance Wars games, considered the best games for the GBA according to MobyGames. Graphically, they are very simple games. The American version of Advance Wars 2 (a hack of which with Chinese menus ships with the console) is somewhat playable. The American version of Advance Wars 1 works a tiny bit worse but is still playable. The European version of Advance Wars 1 (included with the console) performs too bad to be fun to play. The European Advance Wars 2 is basically unplayable because it's too slow. There is no PAL or NTSC version of the GBA or its games. They're always supposed to run at 60 fps. Using TV output doesn't improve the performance either, no matter if PAL or NTSC. Performance on all Advance Wars games gets even worse when there is any dialogue on screen.

3D games on the GBA don't work well either. Of the five Need for Speed games for example (none of which are included), only Porsche (both regions) and Underground 2 will get past the language selection and the EA logo.


## Resources

Note: There are no language strings on the GB300, just a few images. Everything that is not in an image is hardcoded in English. For example, this applies for the error if your `save` subfolder is missing, both low battery warnings and of course the "Loading ..." screen.


### Fonts

There is only one file, `yahei_Arial.ttf`, identical to the SF2000's font file of the same file name. _Microsoft YaHei_ is a Chinese typeface that you can probably find on your computer. Despite also showing up as _Microsoft YaHei_ when you open it, `yahai_Arial.ttf` is different as it uses Arial for non-Chinese script, but with some differences to the usual Arial typeface. For example, it does not feature so-called tabular figures (so names are not aligned in the game lists) and the baseline varies significantly between Latin letters, making the font look "wavy".


### Images

Unlike the SF2000, the GB300 supposedly does not have any unused images (not sure about the 'empty battery' screen though). Many have been renamed on the GB300 compared to the SF2000.

| File         | Comp's   | Dim's    | Description | View |
| ------------ | -------- | -------- | ----------- | ---- |
| `appvc.ikb`  | BGRA8888 | 150x214  | "missing image" image, also the white frame for thumbnails | [view](/images/appvc.ikb.png) |
| `bfrjd.odb`  | RGB565   | 640x280  | language selection, Korean selected (the seventh item) | [view](/images/bfrjd.odb.png) |
| `bisrv.nec`  | RGB565   | 640x480  | pause menu, third entry selected | [view](/images/bisrv.nec.png) |
| `bttlve.kbp` | BGRA8888 | 60x144   | 6 battery states | [view](/images/bttlve.kbp.png) |
| `bxvtb.sby`  | BGRA8888 | 192x224  | "TV SYSTEM" in 7 different languages | [view](/images/bxvtb.sby.png) |
| `c1eac.pal`  | BGRA8888 | 26x22    | checked checkbox, indicating the selected TV standard and language | [view](/images/c1eac.pal.png) |
| `d2d1.hgp`   | RGB565   | 640x480  | pause menu, second entry selected | [view](/images/d2d1.hgp.png) |
| `dism.cef`   | RGB565   | 640x480  | pause menu, first entry selected | [view](/images/dism.cef.png) |
| `dpskc.ctp`  | RGB565   | 384x320  | 4 different selected save states | [view](/images/dpskc.ctp.png) |
| `drivr.ers`  | BGRA8888 | 152x160  | the five words of the pause menu in Arabic | [view](/images/drivr.ers.png) |
| `dsuei.cpl`  | BGRA8888 | 152x160  | the five words of the pause menu in English | [view](/images/dsuei.cpl.png) |
| `dufdr.cwr`  | BGRA8888 | 240x168  | "SETTING" in 7 different languages | [view](/images/dufdr.cwr.png) |
| `dxva2.nec`  | RGB565   | 640x288  | keyboard for search, embossed keys | [view](/images/dxva2.nec.png) |
| `ectte.bke`  | BGRA8888 | 1200x120 | 12 bottom tab items, default state | [view](/images/ectte.bke.png) |
| `eknjo.ofd`  | RGB565   | 640x280  | language selection, Spanish selected (the fifth item) | [view](/images/eknjo.ofd.png) |
| `exaxz.hsp`  | BGRA8888 | 448x768  | logos in the top left | [view](/images/exaxz.hsp.png) |
| `fhshl.skb`  | RGB565   | 640x280  | language selection, English selected (the first item) | [view](/images/fhshl.skb.png) |
| `fixas.ctp`  | BGRA8888 | 152x160  | the five words of the pause menu in Chinese | [view](/images/fixas.ctp.png) |
| `gpapi.bvs`  | RGB565   | 640x480  | pause menu, fifth entry selected | [view](/images/gpapi.bvs.png) |
| `hctml.ers`  | RGB565   | 320x2256 | 6 images of the device with one of the shoulder and ABXY buttons highlighted | [view](/images/hctml.ers.png) |
| `hlink.bvs`  | RGB565   | 640x288  | keyboard for search, keys with a bright frame | [view](/images/hlink.bvs.png) |
| `icuin.cpl`  | BGRA8888 | 152x160  | the five words of the pause menu in Russian | [view](/images/icuin.cpl.png) |
| `igc64.dll`  | BGRA8888 | 217x37   | Yes/No, No highlighted | [view](/images/igc64.dll.png) |
| `irftp.ctp`  | BGRA8888 | 152x160  | the five words of the pause menu in Korean | [view](/images/irftp.ctp.png) |
| `jccatm.kbp` | RGB565   | 640x480  | empty battery screen | [view](/images/jccatm.kbp.png) |
| `jsnno.uby`  | BGRA8888 | 240x168  | "HISTORY" in 7 different languages | [view](/images/jsnno.uby.png) |
| `kmbcj.acp`  | BGRA8888 | 288x448  | "Archive already exists, overwrite this archive?" in 7 different languages | [view](/images/kmbcj.acp.png) |
| `lf9lb.cut`  | RGB565   | 640x280  | language selection, Portuguese selected (the sixth item) | [view](/images/lf9lb.cut.png) |
| `lk7tc.bvs`  | BGRA8888 | 52x192   | key names (B, TB, C, TC, ST, ST, SL, SL, U, U, D, D, L, TL, R, TR, A, TA, Z, TZ, X, TX, Y, TY) | [view](/images/lk7tc.bvs.png) |
| `mczwq.ikb`  | RGB565   | 640x336  | 6 device logos (the GB/GBC share one) for the top of the pause menu; whenever you press the DOWN key in the pause menu, it the image is shown or hidden depending on whether you are at the bottom (Joystick) of not | [view](/images/mczwq.ikb.png) |
| `mhg4s.ihg`  | RGB565   | 400x192  | Background for confirmation messages, with 3 different buttons selected (English only) | [view](/images/mhg4s.ihg.png) |
| `mksh.rcv`   | RGB565   | 640x288  | keyboard for search | [view](/images/mksh.rcv.png) |
| `ntrcq.oba`  | BGRA8888 | 240x168  | "SEARCH" in 7 different languages | [view](/images/ntrcq.oba.png) |
| `nvinf.hsp`  | BGRA8888 | 1200x120 | 12 bottom tab items, selected state | [view](/images/nvinf.hsp.png) |
| `okcg2.old`  | BGRA8888 | 24x24    | star (for favorited games) | [view](/images/okcg2.old.png) |
| `ouenj.dut`  | BGRA8888 | 240x168  | "FAVORITES" in 7 different languages | [view](/images/ouenj.dut.png) |
| `pwsso.occ`  | RGB565   | 640x480  | pause menu, fourth entry selected | [view](/images/pwsso.occ.png) |
| `qasfc.bel`  | BGRA8888 | 328x224  | "Favorites are full !" (when already having 1000) in 7 different languages | [view](/images/qasfc.bel.png) |
| `qdbec.ofd`  | BGRA8888 | 240x168  | "DOWNLOAD ROMS" in 7 different languages | [view](/images/qdbec.ofd.png) |
| `qwave.bke`  | BGRA8888 | 152x160  | the five words of the pause menu in Portuguese | [view](/images/qwave.bke.png) |
| `sdclt.occ`  | BGRA8888 | 424x58   | selection background | [view](/images/sdclt.occ.png) |
| `sfcdr.cpl`  | RGB565   | 640x480  | main background | [view](/images/sfcdr.cpl.png) |
| `sgotd.cwt`  | RGB565   | 640x280  | TV system selection, NTSC selected (first item) | [view](/images/sgotd.cwt.png) |
| `snbqj.uby`  | RGB565   | 640x280  | TV system selection, PAL selected (second item) | [view](/images/snbqj.uby.png) |
| `t2act.sgf`  | RGB565   | 640x280  | language selection, Chinese selected (the second item) | [view](/images/t2act.sgf.png) |
| `tvctu.uby`  | RGB565   | 640x280  | language selection, Arabic selected (the third item) | [view](/images/tvctu.uby.png) |
| `ucby4.aax`  | BGRA8888 | 448x224  | "Folder is empty!" in 7 different languages | [view](/images/ucby4.aax.png) |
| `urlkp.bvs`  | BGRA8888 | 328x224  | "Remove from favorites?" in 7 different languages | [view](/images/urlkp.bvs.png) |
| `vdaz5.bjk`  | RGB565   | 640x280  | language selection, Russian selected (the fourth item) | [view](/images/vdaz5.bjk.png) |
| `wshrm.nec`  | BGRA8888 | 217x37   | Yes/No, Yes highlighted | [view](/images/wshrm.nec.png) |
| `wtrxj.lbd`  | BGRA8888 | 192x224  | "LANGUAGE" in 7 different languages | [view](/images/wtrxj.lbd.png) |
| `xajkg.hsp`  | BGRA8888 | 152x160  | the five words of the pause menu in Spanish | [view](/images/xajkg.hsp.png) |
| `xjebd.clq`  | BGRA8888 | 448x224  | "No games match the keyword!" in 7 different languages | [view](/images/xjebd.clq.png) |
| `zaqrc.olc`  | BGRA8888 | 8x224    | message box left/right border | [view](/images/zaqrc.olc.png) |
| `ztrba.nec`  | RGB565   | 64x320   | key names (single and prefixed with "T" for autofire; also without and with a whitish background) | [view](/images/ztrba.nec.png) |


### ROM Lists

Your custom ROMs are alphabetically indexed into `tsmfk.tax` when the device boots. The following files contain the hardcoded names of the stock ROMs:

| DefaultFolder | File name file | Chinese name file | Pinyin name file |
| ------------- | -------------- | ----------------- | ---------------- |
| **FC**        | `rdbui.tax`    | `fhcfg.nec`       | `nethn.bvs`      |
| **PCE**       | `urefs.tax`    | `adsnt.nec`       | `xvb6c.bvs`      |
| **SFC**       | `scksp.tax`    | `setxa.nec`       | `wmiui.bvs`      |
| **MD**        | `vdsdc.tax`    | `umboa.nec`       | `qdvd6.bvs`      |
| **GB**        | `pnpui.tax`    | `wjere.nec`       | `mgdel.bvs`      |
| **GBC**       | `vfnet.tax`    | `htuiw.nec`       | `sppnp.bvs`      |
| **GBA**       | `mswb7.tax`    | `msdtc.nec`       | `mfpmp.bvs`      |

The Pinyin name file contains a Latin transcription of the Chinese names, but without vowels (except for Latin vowels in the Chinese names, e.g. FIFA). It is used for searching when language is set to Chinese. File names are relative to a folder that can be changed by editing `Foldername.ini`. The first row in the table above refers to the fourth row in `Foldername.ini` (which is `FC` by default) and so on. See the `Foldername.ini` section right below.

In internally, the above table continues with the _file name_ files `tsmfk.tax`, `Favorites.bin` and `History.bin`. The latter two however are in a different format. There are no corresponding Chinese name files or Pinyin name files.

`tsmfk.tax` and the files in the table have the following format:
* 1 Int32 for the number of items.
* Now comes an array, consisting of Int32's for the position of the name inside this file.
* After that, you have all the strings, encoded in UTF-8 (without BOM of course). Each string is terminated with a NUL (`\0`), including the last string, meaning these files always end with a NUL.
* Do note that the order of the two arrays does **not** necessarily match! I copied over 2300 VTxx files to my device and the second half of the strings came before the first half (they weren't exactly halves) and some items were logically ordered (e.g. ignoring case) instead of binarily, probably because Windows put them there in logical order. I believe the default files' order of the arrays does match though.


### Foldername.ini

`Foldername.ini` is neither an INI file, nor does it contain only folder names. It is a general menu configuration. Its default content is:

```
GB300
7
FFFFFF
FF8000 FC
FF8000 PCE
FF8000 SFC
FF8000 MD
FF8000 GB
FF8000 GBC
FF8000 GBA
FF8000 ROMS
FF8000 ROMS
FF8000 ROMS
12 0 3
7 8 9 10 11
20 112 144 208
424 58

```

The file's content is matched to a Format string to extract the values. Funnily, this file is Windows (CR+LF) by default, even though the Format string is specifically Unix (LF). The encoding is UTF-8 without BOM (the device will not get past the boot logo if a BOM is present).

Let's take a closer look at it:

| Default&nbsp;Content | Description |
| -------------------- | ----------- |
| `GB300`              | Name of the device. The BIOS is hardcoded to expect this header. Do not change. |
| `7`                  | Number of languages. It affects how the language strings are loaded from the images with multiple languages inside, and is also the index of the first TV system setup item, counted starting from 0. So you can hide languages from the language menu if you move the TV System backgrounds to their image files. You better leave this. |
| `FFFFFF`             | Default UI text color (HTML Code: `#FFFFFF`) unless selected (see below). All colors in this file are case-insensitive. |
| `FF8000 FC`          | First folder and its selected color (HTML Code: `#FF8000`) |
| `FF8000 PCE`         | Second folder and its selected color |
| `FF8000 SFC`         | Third folder and its selected color |
| `FF8000 MD`          | Fourth folder and its selected color |
| `FF8000 GB`          | Fifth folder and its selected color |
| `FF8000 GBC`         | Sixth folder and its selected color |
| `FF8000 GBA`         | Seventh folder and its selected color |
| `FF8000 ROMS`        | Eighth folder (see below) and its selected color |
| `FF8000 ROMS`        | Ninth folder (see below) and its selected color |
| `FF8000 ROMS`        | Tenth folder (see below) and its selected color |
| `12 0 3`             | Number of bottom tabs (you better leave this), left tab (starting with 0) and default tab starting from the left tab (starting from 0) – so the selected tab is the sum of the latter two numbers (starting from 0)
| `7 8 9 10 11`        | Index of the "ROMS", "FAVORITES", "HISTORY", "SEARCH" and "SETTINGS" tabs on the bottom tab bar (counting from 0). See below. |
| `20 112 144 208`     | Position of the thumbnail (X, Y) and its width and height. Note that this does not cause the image to be stretched, so the latter two also affect the dimensions the device expects and loads from thumbnailed files. This means that decreasing the _height_ may make sense (clips off the bottom part of the image) whereas any other change to the last two numbers will glitch the thumbnails unless you recreate _all_ thumbnailed files. The width and height of `appvc.ikb` must be 6 more each. |
| `424 58`             | Dimensions of `sdclt.occ`. As with the thumbnail file above, this changes the dimensions the device expects. |
|                      | Trailing line feed |

Note that the ROM List files (see the sections above) are bound to the folders in this file. So `rdbui.tax`, which is used for the FC by default, will always refer to the first folder listed here. So if you swap the order of the folders, you need to swap (or rebuild) these files.

Changing anything in the `7 8 9 10 11` row has a lot of strange side effects: The numbers `7`, `8` and `9` each correspond to a file, `tsmfk.tax`, `Favorites.bin` and `History.bin` respectively. The file that corresponds to the first number in this row is populated with the folder given in its corresponding the line above. Example: Say this line starts with `9`, meaning tenth folder and `History.bin`. If you turn on the device, `History.bin` gets populated with file names from the tenth (last) folder defined in `Foldername.ini` because the index `9` refers to `History.bin`. Now it gets inconsistent, because the eighth tab (count from 1) that you just changed (because it's the first number in this line) will look like the History tab now as it has a thumbnail image, but it will still use the `tsmfk.tax` ROM list and still use the eighth image from `ectte.bke`. Accessing the tab that originally was the History however will freeze the device because it too will load its original file, `History.bin`, which has just been populated with data in an unsupported format. Swapping `8` and `9` causes _existing_ Favorites and History lists to be _initially_ swapped, but from then being updated with the correct entries. So it seems that this modification works, so does swapping `10` and `11`. Changing `Foldername.ini` does not affect the order of the bottom tab bar - it's still the same as in the images. tl;dr: You better leave this line. Or better: The entire file except for the colors.


### KeyMapInfo.kmp

The GB300 uses a larger `KeyMapInfo.kmp` file because it stores 7 key mappings instead of just 6. This makes its file incompatible with the SF2000 key map editor. Another difference to the SF2000 is that this file does not exist by default and is only created once you assign non-standard keys.

After each emulated console's key map (24 bytes), it is repeated instantly (probably for the second player). Then comes the next console.

Consoles are encoded in the following order:

| Console/Order    | Physical Button Save Order   | Available Values per Physical Button                                         |
| ---------------- | ---------------------------- | ---------------------------------------------------------------------------- |
| 1. **FC**        | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: FDS Turn Disk, `0x0B00`: FDS Eject/Insert|
| 2. **PCE**       | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: I, `0x0000`: II<!--, `0x0A00`: X, `0x0B00`: Y, `0x0100`: C, `0x0900`: Z--> |
| 3. **SFC**       | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: X, `0x0B00`: Y, `0x0900`: L, `0x0100`: R |
| 4. **MD/SMS**    | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: X, `0x0B00`: Y, `0x0100`: C, `0x0900`: Z<br>(SMS: `0x0000`: 1, `0x0100`: 2) |
| 5. **GB/GBC**    | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B                                                     |
| 6. **GBA**       | `L`, `R`, `X`, `A`, `B`, `Y` | `0x0800`: A, `0x0000`: B, `0x0A00`: L, `0x0B00`: R<!--, `0x0100`: L, `0x0900`: R--> |
| 7. unknown       |                              | defaults identical to SFC's defaults                                         |

After each button's 16-bit value from the table above comes a 16-bit flag for autofire: `0x0100` (or any odd number) if autofire is active (indicated by a `T` in the console's key map editor), `0x0000` (or any even number) if not.

Note that not all of the button values you can select in the device's key map editor actually exist on the PCE and GBA. Assigning other values than those above does display a different text, but doesn't usually give any result. Just the Game Boy treats most (but not all) values assigned to the R button as B.

Note that the table above describes the _actual_ behavior, whereas the key map editor is bogus for a lot of reasons:
* In the editor's optical representation, the _physical_ R and L buttons are swapped for all emulators but GBA (see below) when you use the console's key map editor. If the button on the left (next to the five menu items where you selected "Joystick") is highlighted, you'll set the button physically labeled R.
* To account for this bug, L and R _values_ are swapped for the SFC.
* For GBA, the editor's optical representation of the _physical_ L is swapped with X, and R is swapped with Y.
* To account for this bug, L and R _values_ are swapped with X and Y values respectively for the GBA.

This means that the bugs cancel each other out when the _default_ key mappings are set, so the default mapping seemingly makes sense. Changing the buttons however will rarely ever do what you expect.

Per-game key mappings do not seem to work.

#### multicore Key Maps

multicore is affected by the stock GBA's mapping, so it is still the sixth entry in the file, the physical button save order is still `L`, `R`, `X`, `A`, `B`, `Y` and the bugs described above still make the GB300's key map editor worthless.

Here are a few examples:

| Core      | Emulator        | Con-<br>sole | `0800`<br>(A) | `0000`<br>(B) | `0900`<br>(X) | `0100`<br>(Y) | `0A00`<br>(L) | `0B00`<br>(R) | Test ROM                                                                         |
| --------- | --------------- | ------------ | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | -------------------------------------------------------------------------------- |
| `msx`     | **blueMSX**     | CV           | L             | R             | 1             | 2             | 4             | 3             | Final Test Cartridge                                                             |
| `msx`     | **blueMSX**     | SG           | L             | R             |               |               |               |               | SG-1000 M2 Check Program Proto                                                   |
| `amstradb`| **Caprice32**   | CPC          | Joy1          | Joy2          |               |               |               |               | [Amstrad Diagnostics 1.3](https://github.com/llopis/amstrad-diagnostics)         |
| `amstrad` | **CrocoDS**     | Joy          | Joy1          | Joy2          | Key2          | Key3          |               |               | [Amstrad Diagnostics 1.3](https://github.com/llopis/amstrad-diagnostics)         |
| `amstrad` | **CrocoDS**     | Key          | Space         | Key1          | Key2          | Key3          |               |               | [Amstrad Diagnostics 1.3](https://github.com/llopis/amstrad-diagnostics)         |
| `amstrad` | **CrocoDS**     | int.         | A             | B             | X             | Y             | L             | R             | none                                                                             |
| `amstrad` | **CrocoDS**     | BAS          | X             | Z             | é             | "             |               |               | none                                                                             |
| `col`     | **Gearcoleco**  | CV           | R             | L             | 2             | 1             | 3             | 4             | Final Test Cartridge                                                             |
| `gg`      | **Gearsystem**  | SG           | R             | L             |               |               |               |               | SG-1000 M2 Check Program Proto                                                   |
| `gg`      | **Gearsystem**  | GG           | 2             | 1             |               |               |               |               | [Gamegear button test](https://www.smspower.org/Homebrew/GamegearButtonTest-GG)  |
| `gba`     | **gpSP**        | GBA          | A             | B             | TA            | TB            | L             | R             | [GBA D-Pad Test](https://www.romhacking.net/homebrew/142/)                       |
| `sega`    | **Picodrive**   | MD           | C             | B             | Y             | A             | X             | Z             | Contra - Hard Corps                                                              |
| `sega`    | **Picodrive**   | SG*          | R             | L             |               |               |               |               | SG-1000 M2 Check Program Proto                                                   |
| `sega`    | **Picodrive**   | SMS          | 2             | 1             |               |               |               |               | [SMS Test Suite v0.35](https://github.com/sverx/SMSTestSuite/releases/tag/v0.35) |
| `pokem`   | **PokeMini**    | PM           | A             | B             | TA            |               |               | C             | Zany Cards (no test ROM available)                                               |
| `snes`    | **Snes9x 2005** | SFC          | A             | B             | X             | Y             | L             | R             | NTF 2.5 Test Cartridge                                                           |
| `snes02`  | **Snes9x 2002** | SFC          | A             | B             | X             | Y             | L             | R             | NTF 2.5 Test Cartridge                                                           |

`T` indicates autofire. Homebrew test ROMs are linked, official and/or commercial ones are not. Platforms with an asterisk are not officially supported. Only if you are using the _default_ GBA key mapping, the hex numbers above correspond to the _physical_ buttons in brackets.

Notes:
* Only officially-assignable button values are listed because the values between `0x0200` and `0x0700` either did nothing or counted as `0x0000`, depending on the emulator, assigned value and physical button. Only `B`, `R` and `X` had the chance of counting as `0x0000` whereas the others seemed to always be useless. This was tested with `gba`, `pokem` and `sega`.
* `pokem`: Select is reset.
* There is a [GG button test ROM](https://www.smspower.org/Homebrew/GamegearButtonTest-GG) on smspower.org, but `gg` and `sega` both think it was SMS, so neither the Start button nor the colors work. (Stock also thinks it's SMS, but that's expected because it doesn't even know GG.) You can force `gg` to GG mode. You can do this with `sega` too, but that doesn't work (GG support is unofficial on `sega` anyway).
* GearSystem cannot run [SMS Test Suite v0.35](https://github.com/sverx/SMSTestSuite/releases/tag/v0.35), v0.29 and probably all other versions of that ROM.
* There is an [SMS test ROM for the MD controller](https://www.smspower.org/Homebrew/MegaDrive6ButtonControllerTest-SMS) which does not register the keys properly on `gg`, `sega` and stock. This ROM is also notable for looking completely different on all of these three: It's blue on stock, grey on `sega` and mixed on `gg`.
* The default key order on SMS actually makes sense because that's how the buttons are aligned. Start is Pause.
* `bluemsx`'s mapping seems to make less sense than the others emulators' for the same platform.
* `col`: Select is #, Start is *. `bluemsx` is the other way around.
* CrocoDS gives you the option to map buttons yourself (but it cannot save this, nor does it load `crocods.opt`). The table lists the defaults for joystick and keyboard mode (joystick is the default), the internal name of the keys when you remap them inside CrocoDS and keys it gives in BASIC or the default shell.
* Pressing Start plus whatever button you assigned `0x0900` to (default: `Y`) shows the keyboard in CrocoDS.

### Sounds

* `c2fkec.pgt`
* `dpnet.dll`
* `help.lis`
* `mfsvr.nkf`
* `nyquest.gdb`
* `oldversion.kbe`
* `pagefile.sys`
* `swapfile.sys`

All files are completely identical and have the same use as on the SF2000 v1.71. See the [SF2000 documentation](https://vonmillhausen.github.io/sf2000/#sounds) for more details.


### Other Files

* `Archive.sys`
* `Favorites.bin`
* `History.bin`

Once again, their format and use is identical to the SF2000.

In theory, you can also add ROMs inside the `ROMS` to `Favorites.bin` and `History.bin` (first word = `0x0700`). But because the order changes whenever you add a game with a name lexically lower than any of your favorites, this will change the reference, so the GB300 does not add your own games there (I would guess the SF2000 doesn't do that either).


## List of ROMs

For the FC, MD, GB, GBC, GBA and SFC see [vonmillhausen's list](https://vonmillhausen.github.io/sf2000/defaultRoms/defaultRomsNoIntroCheck.htm). Games that are missing in the GB300 are listed above in this document. There are no new games for consoles that the SF2000 has, but [Final Knockout](https://datomatic.no-intro.org/index.php?page=show_record&s=49&n=0816) does work on the GB300.

To play your own games, create the folder `Roms` on the TF card. You can also use ZIP files to save space. Make sure to create a `save` subfolder to be able to save.


### List of PCE Games

The following is a list of all PCE games that ship with the GB300. The names listed here are the names on No-Intro. Their catalog knows _all_ of the hashes, meaning that the PCE games no strange hacks like many of the other ROMs, especially FC. Thumbnails use only covers here, whereas other consoles use at least some screenshots instead. If you remove everything in brackets (and trailing spaces resulting hereof) from the No-Intro name, you get the name on the GB300, with four exceptions given in non-bold brackets after the No-Intro name.

| No-Intro Name | CRC32 | No-Intro # | No-Intro Status |
| ------------- | ----- | ---------- | --------------- |
| **21 Emon - Mezase Hotel Ou!! (Japan)** | `73614660` | [0002](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0002) | good |
| **1943 Kai (Japan)** | `fde08d6d` | [0001](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0001) | verified |
| **Adventure Island (Japan)** | `8e71d4f3` | [0010](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0010) | verified |
| **Aero Blasters (USA)** | `b03e0b32` | [0012](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0012) | not verified |
| **After Burner II (Japan)** | `ca72a828` | [0013](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0013) | verified |
| **Air Zonk (USA)** | `933d5bcc` | [0014](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0014) | good |
| **Alien Crush (USA)** | `ea488494` | [0016](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0016) | good |
| **All Star Power League (Japan)** | `04a85769` | [0286](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0286) | not verified |
| **Andre Panza Kick Boxing (USA)** | `a980e0e9` | [0264](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0264) | good |
| **Aoi Blink (Japan)** | `08a09b9a` | [0018](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0018) | verified |
| **Appare! Gateball (Japan)** | `2b54cba2` | [0019](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0019) | verified |
| **Armed F (Japan)** | `20ef87fd` | [0020](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0020) | verified |
| **Artist Tool (Japan)** | `5e4fa713` | [0021](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0021) | not verified |
| **Atomic Robo-Kid Special (Japan)** | `dd175efd` | [0022](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0022) | verified |
| **Ballistix (USA)** | `420fa189` | [0025](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0025) | not verified |
| **Bari Bari Densetsu (Japan)** | `c267e25d` | [0026](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0026) | verified |
| **Barunba (Japan)** | `4a3df3ca` | [0027](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0027) | verified |
| **Batman (Japan)** | `106bb7b2` | [0028](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0028) | good |
| **Battle Lode Runner (Japan)** | `59e44f45` | [0029](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0029) | verified |
| **Battle Royale (USA)** | `e70b01af` | [0030](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0030) | not verified |
| **Benkei Gaiden (Japan) (Wii Virtual Console)** | `c9626a43` | [2465](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=2465) | good |
| **Bikkuriman World (Japan)** | `2841fd1e` | [0034](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0034) | verified |
| **Blazing Lazers (USA)** | `b4a1b0f6` | [0036](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0036) | not verified |
| **Bloody Wolf (USA)** | `37baf6bc` | [0038](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0038) | good |
| **Bodycon Quest II (Japan) (Unl)** | `ffd92458` | [0039](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0039) | not verified |
| **Bomberman - Users Battle (Japan)** | `1489fa51` | [0046](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0046) | not verified |
| **Bomberman '93 - Special Version (Japan)** | `02309aa0` | [0041](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0041) | good |
| **Bomberman '93 (USA)** | `56171c1c` | [0042](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0042) | not verified |
| **Bomberman '94 (Japan)** | `05362516` | [0043](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0043) | verified |
| **Bomberman (USA)** | `5f6f3c2a` | [0045](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0045) | not verified |
| **Bonk 3 - Bonk's Big Adventure (USA)** | `5a3f76d8` | [0047](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0047) | not verified |
| **Bonk's Adventure (USA)** | `599ead9b` | [0048](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0048) | not verified |
| **Bonk's Revenge (USA) (Alt 2)** | `14250f9a` | [0429](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0429) | not verified |
| **Bouken Danshaku Don - The Lost Sunheart (Japan)** | `8f4d9f94` | [0050](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0050) | good |
| **Boxyboy (USA)** | `605be213` | [0051](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0051) | not verified |
| **Bravoman (USA)** | `cca08b02` | [0052](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0052) | not verified |
| **Break In (Japan)** | `c9d7426a` | [0053](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0053) | verified |
| **Bubblegum Crash! - Knight Sabers 2034 (Japan)** | `0d766139` | [0054](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0054) | verified |
| **Bull Fight - Ring no Hasha (Japan)** | `5c4d1991` | [0055](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0055) | good |
| **Burning Angels (Japan)** | `d233c05a` | [0056](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0056) | good |
| **Busou Keiji - Cyber Cross (Japan)** | `d0c250ca` | [0057](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0057) | good |
| **Cadash (USA)** | `bb0b3aef` | [0059](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0059) | not verified |
| **Champion Wrestler (Japan)** | `9edc0aea` | [0060](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0060) | verified |
| **Champions Forever Boxing (USA)** | `15ee889a` | [0061](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0061) | not verified |
| **Chase H.Q. (USA)** | `9298254c` | [0358](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0358) | not verified |
| **Chew-Man-Fu (USA)** | `8cd13e9a` | [0062](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0062) | good |
| **Chikudenya Toubei - Kubikiri Yakata Yori (Japan)** | `cab21b2e` | [0064](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0064) | verified |
| **China Warrior (USA)** | `a2ee361d` | [0066](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0066) | verified |
| **Circus Lido (Japan)** | `c3212c24` | [0068](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0068) | verified |
| **City Hunter (Japan)** | `f91b055f` | [0069](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0069) | good |
| **Columns (Japan)** | `99f7a572` | [0070](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0070) | verified |
| **Coryoon - Child of Dragon (Japan)** | `b4d29e3b` | [0071](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0071) | good |
| **Cratermaze (USA)** | `9033e83a` | [0073](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0073) | not verified |
| **Cross Wiber - Cyber Combat Police (Japan)** | `2df97bd0` | [0074](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0074) | good |
| **Cyber-Core (USA)** (Cyber Core) | `4cfb6e3e` | [0076](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0076) | not verified |
| **Cyber Dodge (Japan)** | `b5326b16` | [0077](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0077) | verified |
| **Cyber Knight (Japan)** | `a594fac0` | [0078](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0078) | verified |
| **Daichi-kun Crisis - Do Natural (Japan)** | `61a2935f` | [0080](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0080) | verified |
| **Daisenpuu (Japan)** | `9107bcc8` | [0079](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0079) | verified |
| **Darius Alpha (Japan) (SG Enhanced)** | `b0ba689f` | [0081](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0081) | not verified |
| **Darius Plus (Japan) (SG Enhanced)** | `bebfe042` | [0082](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0082) | verified |
| **Darkwing Duck (USA)** | `4ac97606` | [0083](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0083) | not verified |
| **Davis Cup Tennis (USA)** | `9edab596` | [0084](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0084) | not verified |
| **Dead Moon (USA)** | `f5d98b0b` | [0086](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0086) | not verified |
| **Deep Blue (USA)** | `16b40b44` | [0087](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0087) | not verified |
| **Detana!! TwinBee (Japan)** | `5cf59d80` | [0089](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0089) | verified |
| **Devil's Crush (USA)** | `157b4492` | [0091](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0091) | good |
| **Die Hard (Japan)** | `1b5b1cb1` | [0092](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0092) | good |
| **Digital Champ (Japan)** | `17ba3032` | [0093](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0093) | verified |
| **Don Doko Don! (Japan)** | `f42aa73e` | [0094](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0094) | verified |
| **Doraemon - Nobita no Dorabian Night (Japan)** | `013a747f` | [0096](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0096) | good |
| **Double Dungeons (USA)** | `4a1a8c60` | [0098](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0098) | not verified |
| **Down Load (Japan)** (Download) | `85101c20` | [0099](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0099) | verified |
| **Dragon Egg! (Japan)** | `442405d5` | [0101](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0101) | verified |
| **Dragon Saber (Japan)** | `3219849c` | [0102](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0102) | verified |
| **Dragon Spirit (USA)** | `086f148c` | [0105](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0105) | not verified |
| **Dragon's Curse (USA)** | `7d2c4b09` | [0106](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0106) | not verified |
| **Dungeon Explorer (USA)** | `4ff01515` | [0111](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0111) | verified |
| **Dungeons & Dragons - Order of the Griffon (USA)** | `fae0fc60` | [0254](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0254) | not verified |
| **Energy (Japan)** | `ca68ff21` | [0112](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0112) | verified |
| **Eternal City - Toshi Tensou Keikaku (Japan)** | `b18d102d` | [0378](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0378) | good |
| **F1 Circus '91 (Japan)** | `d7cfd70f` | [0115](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0115) | verified |
| **F1 Circus '92 - The Speed of Sound (Japan)** | `b268f2a2` | [0116](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0116) | verified |
| **F1 Circus (Japan)** | `e14dee08` | [0117](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0117) | verified |
| **F-1 Dream (Japan)** | `d50ff730` | [0113](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0113) | good |
| **F-1 Pilot - You're King of Kings (Japan)** | `09048174` | [0114](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0114) | verified |
| **F1 Triple Battle (Japan)** | `13bf0409` | [0119](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0119) | verified |
| **Falcon (USA)** | `0bc0a12b` | [0120](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0120) | not verified |
| **Fantasy Zone (USA)** | `e8c3573d` | [0122](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0122) | not verified |
| **Fighting Run (Japan)** | `1828d2e5` | [0123](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0123) | verified |
| **Final Blaster (Japan)** | `c90971ba` | [0124](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0124) | good |
| **Final Lap Twin (USA)** | `26408ea3` | [0126](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0126) | verified |
| **Final Match Tennis (Japan)** | `560d2305` | [0127](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0127) | verified |
| **Final Soldier - Special Version (Japan) (En)** | `02a578c5` | [0129](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0129) | good |
| **Final Soldier (Japan) (En)** | `af2dd2af` | [0128](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0128) | verified |
| **Fire Pro Wrestling - Combination Tag (Japan)** | `90ed6575` | [0130](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0130) | verified |
| **Fire Pro Wrestling 2 - 2nd Bout (Japan)** | `e88987bb` | [0131](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0131) | verified |
| **Fire Pro Wrestling 3 - Legend Bout (Japan)** | `534e8808` | [0132](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0132) | verified |
| **Formation Soccer - Human Cup '90 (Japan)** | `85a1e7b6` | [0133](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0133) | verified |
| **Formation Soccer - On J League (Japan)** | `7146027c` | [0134](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0134) | verified |
| **Fushigi no Yume no Alice (Japan)** | `12c4e6fd` | [0135](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0135) | verified |
| **Gai Flame (Japan)** | `95f90dec` | [0136](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0136) | verified |
| **Gaia no Monshou (Japan)** | `6fd6827c` | [0137](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0137) | verified |
| **Galaga '88 (Japan)** | `1a8393c6` | [0138](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0138) | verified |
| **Galaga '90 (USA)** | `2909dec6` | [0139](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0139) | not verified |
| **Ganbare! Golf Boys (Japan)** | `27a4d11a` | [0140](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0140) | verified |
| **Gekisha Boy (Japan)** | `e8702d51` | [0141](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0141) | good |
| **Genji Tsuushin Agedama (Japan)** | `ad450dfc` | [0142](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0142) | verified |
| **Genpei Toumaden (Japan)** | `b926c682` | [0143](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0143) | verified |
| **Gokuraku! Chuuka Taisen (Japan)** | `e749a22c` | [0146](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0146) | good |
| **Gomola Speed (Japan)** | `4bd38f17` | [0147](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0147) | verified |
| **Gradius (Japan)** | `0517da65` | [0148](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0148) | verified |
| **Gunboat (USA)** | `f370b58e` | [0149](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0149) | not verified |
| **Gunhed - Special Version (Japan)** | `57f183ae` | [0151](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0151) | good |
| **Gunhed (Japan)** | `a17d4d7e` | [0150](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0150) | verified |
| **Hana Taaka Daka! (Japan)** | `ba4d0dd4` | [0152](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0152) | good |
| **Hanii in the Sky (Japan)** | `bf3e2cc7` | [0153](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0153) | verified |
| **Hanii on the Road (Japan)** | `9897fa86` | [0154](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0154) | verified |
| **Hatris (Japan)** | `44e7df53` | [0155](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0155) | verified |
| **Heavy Unit (Japan)** | `eb923de5` | [0156](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0156) | good |
| **Hisou Kihei X-SERD (Japan)** | `1cab1ee6` | [0157](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0157) | verified |
| **Hit the Ice - VHL - The Official Video Hockey League (USA)** | `8b29c3aa` | [0159](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0159) | not verified |
| **Honoo no Toukyuuji - Dodge Danpei (Japan)** | `b01ee703` | [0160](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0160) | verified |
| **ImageFight (Japan)** (Image Fight) | `a80c565f` | [0162](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0162) | verified |
| **Impossamole (USA)** | `e2470f5f` | [0163](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0163) | not verified |
| **J.J. & Jeff (USA)** | `e01c5127` | [0165](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0165) | not verified |
| **J.League Greatest Eleven (Japan)** | `0ad97b04` | [0164](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0164) | verified |
| **Jack Nicklaus - Championship Golf (Japan)** | `ea751e82` | [0166](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0166) | verified |
| **Jackie Chan's Action Kung Fu (USA)** | `9d2f6193` | [0169](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0169) | not verified |
| **Jaseiken Necromancer (Japan)** | `53109ae6` | [0235](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0235) | verified |
| **Jigoku Meguri (Japan)** | `cc7d3eeb` | [0170](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0170) | good |
| **Jinmu Denshou (Japan)** | `c150637a` | [0171](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0171) | verified |
| **Juuouki (Japan)** | `c8c7d63e` | [0173](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0173) | verified |
| **Kaizou Choujin Shubibinman (Japan)** | `a9084d6e` | [0175](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0175) | verified |
| **Kattobi! Takuhai-kun (Japan)** | `4f2844b0` | [0178](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0178) | good |
| **Keith Courage in Alpha Zones (USA)** | `474d7a72` | [0179](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0179) | good |
| **KickBall (Japan)** | `7e3c367b` | [0180](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0180) | verified |
| **Kiki Kaikai (Japan)** | `c0cb5add` | [0181](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0181) | good |
| **King of Casino (USA)** | `2f2e2240` | [0183](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0183) | not verified |
| **Klax (USA)** | `0f1b59b4` | [0185](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0185) | not verified |
| **Knight Rider Special (Japan)** | `c614116c` | [0186](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0186) | verified |
| **Kore ga Pro Yakyuu '89 (Japan)** | `44f60137` | [0187](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0187) | verified |
| **Kore ga Pro Yakyuu '90 (Japan)** | `1772b229` | [0188](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0188) | verified |
| **Kung Fu, The (Japan)** | `b552c906` | [0189](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0189) | verified |
| **Kyuukyoku Tiger (Japan)** | `09509315` | [0192](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0192) | verified |
| **Lady Sword - Ryakudatsu Sareta 10-nin no Otome (Japan) (Unl)** | `c6f764ec` | [0193](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0193) | verified |
| **Legend of Hero Tonma (USA)** | `3c131486` | [0196](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0196) | good |
| **Legendary Axe II (USA)** | `220ebf91` | [0197](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0197) | not verified |
| **Legendary Axe, The (USA)** | `2d211007` | [0198](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0198) | not verified |
| **Lode Runner - Lost Labyrinth (Japan)** | `e6ee1468` | [0199](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0199) | verified |
| **Maerchen Maze (Japan)** | `a15a1f37` | [0200](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0200) | verified |
| **Magical Chase (USA)** | `95cd2979` | [0202](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0202) | not verified |
| **Mahjong Gokuu Special (Japan)** | `f8861456` | [0206](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0206) | verified |
| **Mahjong Haouden - Kaiser's Quest (Japan)** | `df10c895` | [0207](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0207) | verified |
| **Mahjong Shikaku Retsuden - Mahjong Wars (Japan)** | `6c34aaea` | [0208](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0208) | verified |
| **Maison Ikkoku (Japan)** | `5c78fee1` | [0209](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0209) | verified |
| **Makai Hakkenden Shada (Japan)** | `be62eef5` | [0211](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0211) | verified |
| **Makai Prince Dorabocchan (Japan)** | `b101b333` | [0212](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0212) | good |
| **Maniac Pro Wres - Asu e no Tatakai (Japan)** | `99f2865c` | [0214](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0214) | verified |
| **Metal Stoker (Japan)** | `25a02bee` | [0216](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0216) | good |
| **Mizubaku Daibouken (Japan)** | `b2ef558d` | [0218](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0218) | verified |
| **Momotarou Densetsu Gaiden - Dai-1-shuu (Japan)** | `f860455c` | [0219](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0219) | verified |
| **Momotarou Densetsu II (Japan)** | `d9e1549a` | [0220](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0220) | verified |
| **Momotarou Densetsu Turbo (Japan)** | `625221a6` | [0221](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0221) | verified |
| **Momotarou Katsugeki (Japan)** | `345f43e9` | [0222](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0222) | verified |
| **Monster Pro Wres (Japan)** | `f2e46d25` | [0223](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0223) | good |
| **Morita Shougi PC (Japan)** | `2546efe0` | [0224](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0224) | good |
| **Moto Roader II (Japan)** | `0b7f6e5f` | [0227](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0227) | verified |
| **Moto Roader (USA)** | `e2b0d544` | [0226](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0226) | good |
| **Battle Chopper (Japan)** (Mr. Heli no Daibouken) | `2cb92290` | [0229](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0229) | verified |
| **Naxat Open (Japan)** | `60ecae22` | [0232](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0232) | verified |
| **Naxat Stadium (Japan)** | `20a7d128` | [0233](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0233) | verified |
| **Nazo no Mascarade (Japan)** | `0441d85a` | [0234](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0234) | good |
| **Necros no Yousai (Japan)** | `fb0fdcfe` | [0236](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0236) | good |
| **Nekketsu Koukou Dodgeball-bu - PC Bangai Hen (Japan)** | `65fdb863` | [0238](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0238) | verified |
| **Nekketsu Koukou Dodgeball-bu - PC Soccer Hen (Japan)** | `f2285c6d` | [0239](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0239) | good |
| **Neutopia II (USA)** | `c4ed4307` | [0243](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0243) | not verified |
| **Neutopia (USA)** | `a9a94e1b` | [0241](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0241) | not verified |
| **New Adventure Island (USA)** | `756a1802` | [0244](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0244) | verified |
| **NewZealand Story, The (Japan)** | `8e4d75a8` | [0245](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0245) | good |
| **NHK Taiga Drama - Taiheiki (Japan)** | `a32430d5` | [0246](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0246) | verified |
| **Night Creatures (USA)** | `c159761b` | [0247](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0247) | not verified |
| **Niko Niko Pun (Japan)** | `82def9ee` | [0248](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0248) | verified |
| **Ninja Ryuukenden (Japan)** | `67573bac` | [0249](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0249) | good |
| **Ninja Spirit (USA)** | `de8af1c1` | [0250](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0250) | good |
| **Ninja Warriors, The (Japan)** | `96e0cd9d` | [0251](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0251) | verified |
| **Obocchama-kun (Japan)** | `4d3b0bc9` | [0252](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0252) | verified |
| **Off the Wall (USA) (Proto)** | `8621ae02` | [0428](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0428) | good |
| **Operation Wolf (Japan)** | `ff898f87` | [0253](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0253) | verified |
| **Ordyne (USA)** | `e7bf2a74` | [0256](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0256) | not verified |
| **Out Live (Japan)** | `5cdb3f5b` | [0257](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0257) | verified |
| **Out Run (Japan)** | `e203f223` | [0258](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0258) | verified |
| **Override (Japan)** | `b74ec562` | [0259](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0259) | good |
| **P-47 - The Freedom Fighter (Japan)** | `7632db90` | [0260](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0260) | verified |
| **Pachio-kun - Juuban Shoubu (Japan)** | `4148fd7c` | [0263](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0263) | verified |
| **Pac-Land (Japan)** | `14fad3ba` | [0261](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0261) | verified |
| **Parasol Stars - The Story of Bubble Bobble III (USA)** | `e6458212` | [0267](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0267) | good |
| **Parodius Da! - Shinwa kara Owarai e (Japan)** | `647718f9` | [0268](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0268) | verified |
| **PC Pachi-Slot - Idol Gambler (Japan) (Unl)** | `0aa88f33` | [0276](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0276) | verified |
| **Populous (Japan)** | `083c956a` | [0277](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0277) | verified |
| **Power Drift (Japan)** | `25e0f6e9` | [0279](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0279) | verified |
| **Power Eleven (Japan)** | `3e647d8b` | [0281](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0281) | verified |
| **Power Gate (Japan)** | `be8b6e3b` | [0282](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0282) | good |
| **Power Golf (USA)** | `ed1d3843` | [0284](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0284) | good |
| **Power League 4 (Japan)** | `30cc3563` | [0290](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0290) | verified |
| **Power League 5 (Japan)** | `8b61e029` | [0291](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0291) | verified |
| **Power League '93 (Japan)** | `7d3e6f33` | [0285](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0285) | good |
| **Power League II (Japan)** | `c5fdfa89` | [0288](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0288) | verified |
| **Power League III (Japan)** | `8aa4b220` | [0289](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0289) | verified |
| **Power League (Japan)** | `69180984` | [0287](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0287) | verified |
| **Power Tennis (Japan)** | `8def5aa1` | [0293](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0293) | verified |
| **Pro Tennis - World Court (Japan)** | `11a36745` | [0294](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0294) | verified |
| **Pro Yakyuu World Stadium '91 (Japan)** | `66b167a9` | [0295](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0295) | verified |
| **Psycho Chaser (Japan)** | `03883ee8` | [0297](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0297) | verified |
| **Psychosis (USA)** | `6cc10824` | [0298](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0298) | not verified |
| **Puzzle Boy (Japan)** | `faa6e187` | [0299](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0299) | good |
| **Puzznic (Japan)** | `965c95b3` | [0300](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0300) | verified |
| **Rabio Lepus Special (Japan)** | `d8373de6` | [0305](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0305) | verified |
| **Racing Damashii (Japan)** | `3e79734c` | [0306](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0306) | verified |
| **Raiden (USA)** | `bc59c31e` | [0308](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0308) | not verified |
| **Rastan Saga II (Japan)** | `00c38e69` | [0309](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0309) | verified |
| **Rock-On (Japan)** | `2fd65312` | [0310](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0310) | verified |
| **R-Type I (Japan)** | `cec3d28a` | [0303](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0303) | verified |
| **R-Type II (Japan) (v1.1)** | `417b961d` | [0304](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0304) | good |
| **R-Type (USA)** | `91ce5156` | [0302](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0302) | good |
| **S.C.I. - Special Criminal Investigation (Japan)** | `09a0bfcc` | [0341](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0341) | verified |
| **Sadakichi Seven - Hideyoshi no Ougon (Japan)** | `f999356f` | [0312](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0312) | verified |
| **Salamander (Japan)** | `faecce20` | [0314](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0314) | verified |
| **Samurai-Ghost (USA)** | `77a924b7` | [0315](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0315) | not verified |
| **Sekigahara (Japan)** | `2e955051` | [0316](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0316) | verified |
| **Sengoku Mahjong (Japan)** | `90e6bf49` | [0317](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0317) | verified |
| **Shanghai (Japan)** | `6923d736` | [0318](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0318) | verified |
| **Shinobi (Japan)** | `bc655cf3` | [0319](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0319) | verified |
| **Shiryou Sensen - War of the Dead (Japan)** | `469a0fdf` | [0320](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0320) | verified |
| **Shockman (USA)** | `2774462c` | [0321](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0321) | good |
| **SideArms (USA)** | `d1993c9f` | [0325](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0325) | good |
| **Silent Debuggers (USA)** | `fa7e5d66` | [0327](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0327) | not verified |
| **Sindibad - Chitei no Daimakyuu (Japan)** | `b5c4eebd` | [0328](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0328) | good |
| **Sinistron (USA)** | `4f6e2dbd` | [0329](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0329) | good |
| **Skweek (Japan)** | `4d539c9f` | [0330](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0330) | verified |
| **Soldier Blade - Special Version (Japan)** | `f39f38ed` | [0333](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0333) | good |
| **Soldier Blade (USA)** | `4bb68b13` | [0332](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0332) | not verified |
| **Somer Assault (USA)** | `8fcaf2e9` | [0334](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0334) | not verified |
| **Son Son II (Japan)** | `d7921df2` | [0335](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0335) | verified |
| **Sonic Spike - World Championship Beach Volleyball (USA)** | `f74e5eb3` | [0336](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0336) | not verified |
| **Space Harrier (USA)** | `43b05eb8` | [0339](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0339) | not verified |
| **Space Invaders - Fukkatsu no Hi (Japan)** | `99496db3` | [0340](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0340) | verified |
| **Spin Pair (Japan)** | `1c6ff459` | [0342](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0342) | good |
| **Spiral Wave (Japan)** | `a5290dd0` | [0343](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0343) | verified |
| **Splatterhouse (USA)** | `d00ca74f` | [0345](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0345) | not verified |
| **Stratego (Japan)** | `727f4656` | [0346](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0346) | verified |
| **Strip Fighter II (Japan) (Unl)** | `d6fc51ce` | [0348](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0348) | good |
| **Super Metal Crusher (Japan)** | `56488b36` | [0349](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0349) | good |
| **Super Momotarou Dentetsu II (Japan)** | `2bc023fc` | [0351](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0351) | verified |
| **Super Momotarou Dentetsu (Japan) (Alt)** | `69d52e7a` | [2464](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=2464) | good |
| **Super Star Soldier (USA)** | `db29486f` | [0353](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0353) | not verified |
| **Super Volleyball (USA)** | `245040b3` | [0355](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0355) | not verified |
| **Susanoou Densetsu (Japan)** | `cf73d8fc` | [0356](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0356) | verified |
| **Takeda Shingen (Japan)** | `f022be13` | [0360](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0360) | verified |
| **Takin' It to the Hoop (USA)** | `e9d51797` | [0362](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0362) | good |
| **TaleSpin (USA)** | `bae9cecc` | [0363](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0363) | not verified |
| **Tatsujin (Japan) (Beta)** | `c1b26659` | [0365](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0365) | not verified |
| **Tatsunoko Fighter (Japan)** | `eeb6dd43` | [0366](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0366) | verified |
| **Ten no Koe Bank (Japan)** | `3b3808bd` | [0367](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0367) | verified |
| **Tenseiryuu - Saint Dragon (Japan)** | `2e278ccb` | [0368](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0368) | verified |
| **Terra Cresta II - Mandoler no Gyakushuu (Japan)** | `1b2d0077` | [0369](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0369) | good |
| **Thunder Blade (Japan)** | `ddc3e809` | [0370](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0370) | verified |
| **Tiger Road (USA)** | `985d492d` | [0371](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0371) | not verified |
| **Time Cruise II (Japan)** | `cfec1d6a` | [0373](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0373) | good |
| **Time Cruise (USA)** | `02c39660` | [0372](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0372) | not verified |
| **Timeball (USA)** | `5d395019` | [0374](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0374) | not verified |
| **Titan (Japan)** | `d20f382f` | [0375](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0375) | verified |
| **Toilet Kids (Japan)** | `53b7784b` | [0376](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0376) | good |
| **Tower of Druaga, The (Japan)** | `72e00bc4` | [0379](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0379) | good |
| **Toy Shop Boys (Japan)** | `97c5ee9a` | [0380](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0380) | verified |
| **Tricky Kick (USA)** | `48e6fd34` | [0382](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0382) | not verified |
| **Tsuppari Oozumou - Heisei Ban (Japan)** | `61a6e210` | [0383](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0383) | good |
| **Tsuru Teruhito no Jissen Kabushiki Bai Bai Game (Japan)** | `f70112e5` | [0384](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0384) | verified |
| **Turrican (USA)** | `eb045edf` | [0385](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0385) | not verified |
| **TV Sports Basketball (USA)** | `ea54d653` | [0387](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0387) | not verified |
| **TV Sports Football (USA)** | `5e25b557` | [0389](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0389) | not verified |
| **TV Sports Hockey (USA)** | `97fe5bcf` | [0391](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0391) | not verified |
| **Valkyrie no Densetsu (Japan)** | `a3303978` | [0403](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0403) | verified |
| **Veigues - Tactical Gladiator (USA)** | `99d14fb7` | [0394](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0394) | not verified |
| **Victory Run (USA)** | `85cbd045` | [0396](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0396) | not verified |
| **Vigilante (USA)** | `79d49a0d` | [0398](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0398) | verified |
| **Volfied (Japan)** | `ad226f30` | [0400](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0400) | verified |
| **Wai Wai Mahjong - Yukai na Janyuu-tachi (Japan)** | `a2a0776e` | [0402](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0402) | verified |
| **Wallaby!! - Usagi no Kuni no Kangaroo Race (Japan)** | `0112d0c7` | [0404](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0404) | verified |
| **Winning Shot (Japan)** | `9b5ebc58` | [0405](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0405) | verified |
| **Wonder Momo (Japan)** | `59d07314` | [0406](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0406) | verified |
| **World Circuit (Japan)** | `b3eeea2e` | [0408](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0408) | verified |
| **World Class Baseball (USA)** | `4186d0c0` | [0409](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0409) | good |
| **World Court Tennis (USA)** | `a4457df0` | [0410](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0410) | not verified |
| **World Jockey (Japan)** | `a9ab2954` | [0411](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0411) | verified |
| **World Sports Competition (USA)** | `4b93f0ac` | [0412](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0412) | not verified |
| **W-Ring - The Double Rings (Japan)** | `be990010` | [0401](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0401) | good |
| **Xevious - Fardraut Saga (Japan)** | `f8f85eec` | [0413](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0413) | verified |
| **Yo, Bro (USA)** | `3ca7db48` | [0414](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0414) | not verified |
| **Youkai Douchuuki (Japan)** | `f131b706` | [0427](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0427) | verified |
| **You-You Jinsei (Japan)** | `c0905ca9` | [0416](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0416) | verified |
| **Zero4 Champ (Japan) (Alt)** (Zero4 Champ V1.5) | `b77f2e2f` | [0418](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0418) | not verified |
| **Zero4 Champ (Japan)** | `ee156721` | [0417](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0417) | verified |
| **Zipang (Japan)** | `67aab7a1` | [0419](https://datomatic.no-intro.org/index.php?page=show_record&s=12&n=0419) | good |
