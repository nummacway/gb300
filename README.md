# GB300

The GB300 is a cheap handheld that emulates video game consoles. The cheapest way to get it is via [AliExpress "Pick 3 and Save"](https://www.aliexpress.com/gcp/300000512/nnmixupdatev3?productIds=1005006436983834) for $9.99 (_not_ a referral link!) if you can find two more items from that page (or simply buy three GB300!). At the start of each month and some "shopping festivals", there is a 10% discount, so you can get it for $8.99. That's a few dollars more than the cheapest game consoles on AliExpress often dubbed the Famiclones, but the GB300 offers eight classic consoles (instead of just the Famicon), comes with _way_ more games (even on the Famicon), you can add your own games, and you can save (states and sometimes standard GBA battery saves).

Some see it as a clone of the (usually a bit more expensive) Data Frog SF2000, which however is a bit different. Because the [SF2000 has already been documented](https://vonmillhausen.github.io/sf2000/), this page focusses primarily on the differences.

This document is work in progress. Feel free to contribute by contacting `numma_cway` on Discord, creating a fork and pull request, or by opening an issue on Github (see link at the top of this page). If you have any questions, feel free to join the `#data_frog_sf2000` channel on the [Retro Handhelds Discord](https://discord.gg/retrohandhelds). There is also a [`Gb300 dev` thread](https://discord.com/channels/741895796315914271/1195581037003165796) on that Discord.


## Hardware

The hardware seems somewhat similar to the SF2000. The most important difference is the vertical form factor which makes the device look a bit like the (much heavier) Game Boy Color. The GB300 lacks the SF2000's "digital analog stick" and the buttons feel somewhat cheap.

The screen is a cheap LCD screen compared to the SF2000’s IPS screen. The horizontal angle (sideways) is extremely small, but vertical is alright. Especially when playing dark games in a dark room, the very bright black is an issue, as neither device has a brightness control. People who love the GB300 for its form factor and straight-forward interface have bought an SF2000 just to [swap its screen into the GB300](https://discord.com/channels/741895796315914271/1197607372277940314), so the rest of the device can't be that bad, hmm? The GB300's default screen has diagonal(!) screen tearing.

Unlike the SF2000, the GB300 has a working volume control.

Because it lacks arcade support accounting for 2.75 GB on the SF2000, the device ships with only a 8 GB TF/microSDHC card (42 MB of which not allocated to a partition), formatted FAT32. It includes the firmware and the default set of 6267 ROMs. This leaves around 1.75 GB for your own ROMs. Actually, there's more space if you follow the manual: All the ROMs are just for demonstration and you are supposed to delete them right when you receive the console, even though the menus are hardcoded to exactly these files.

The device comes with a 70&thinsp;cm (28") cable from a 2.5mm male audio jack to two male RCA (cinch) jacks. The yellow RCA jack is for composite video and the red one for sound. You can plug them into older TVs either directly or via a SCART adapter. If you plug the cable in the GB300, its own screen will be turned off. The TV output has a better resolution (640x480) than the internal screen's 320x240. If your TV doesn't care, use NTSC 480i to prevent unnecessary vertical scaling to 576i. NTSC outputs a vertically pixel-perfect result of the interface. Unlike the SF2000, the TV signal will be fine while charging the GB300.

There is currently no information if you can use an external gamepad. For comparison, the SF2000 accepts both, wireless and wired gamepads that ship with some similar consoles. Neither have any use with normal hardware like computers and cannot normally be bought individually. Some sellers began bundling the SF2000 with the wireless gamepad from the SF900, a TV game stick. If you see an SF2000 offer on AliExpress that looks way too cheap, that's because of an option to just buy a wireless gamepad, but compatibility with the GB300 is unknown.


## General Firmware features

The GB300 emulates the following devices:
* Nintendo Entertainment System (Famicon)
* PC Engine (Turbografx-16)
* Super Nintendo Entertainment System (Super Famicon)
* SEGA Master System (SEGA Mark III)
* SEGA Mega Drive (SEGA Genesis)
* Game Boy
* Game Boy Color
* Game Boy Advance

Compared to the SF2000 stock firmware, the GB300 lacks the arcade section and adds the PCE.

The SF2000 firmware does not work on the GB300. There is no known way to retrieve an updated firmware because the manufacturer is unknown, so the only chance will be to wait for an alternative firmware to be released. The BIOS dates to the 15th of December, 2023.

There's actually two things called Firmware on the GB300. There is a small bootloader that loads the firmware from the SD card. You should [patch the bootloader](https://vonmillhausen.github.io/sf2000/#bootloader-bug). Really. This thing works for the GB300 as well.


### Saving

The device features four save _states_ per game which allow saving at any point (press Start+Select). However, they are usually incompatible between different emulators. If you want to try anyway, you first need to extract them from their `zlib`-based format (same as on the SF2000). There is [a tool for that](https://vonmillhausen.github.io/sf2000/tools/saveStateTool.htm). Tests with VBA-M's save states (after extracting the ZIP file that is VBA-M's save state format) didn't work (black screen on the GB300).

Normally, you would be able to exchange _battery_ files between emulators. These are the files that store the savegames created by the games' save feature. However, there's an issue with them on the GB300: To my knowledge, battery files do not work at all for all platforms but the GBA. So the rest of this paragraph is about the GBA only. If you want to _load_ a battery file from another emulator, place it in the `Roms` folder (not the `save` subfolder) - even for stock ROMs! Saving is a bit more complicated. Sometimes it works, sometimes it doesn't. And even if you can load a battery before turning off the device, this does not guarantee that you will still be able to load after turning off the device. Saving and loading between switching off the GB300 in the meantime seems to work, so games that force you to save and restart (e.g. Pokémon games) should work, at least on the GBA. Should you need to get your battery file from the device, load your state and save. Repeat until you can load your battery after restarting the GB300. Then you should have a working battery.


## ROMs and Gameplay

To play your own games, create the folder `Roms` (case-insensitive) on the TF card and put your ROMs there. You can also use single-file ZIP files to save memory.

Stock ROMs however come in their own format. The first `0xEA00` bytes are a 144x208 pixel RGB565 images with no header whatsoever. After that comes a ZIP file, with four differences to keep you from opening it:
* The magic numbers are different. There are three in a standard ZIP file with one file.
  * The local block header magic number is `0x57515703` but should be `0x504B0304`.
  * The central directory header magic number is `0x57515702` but should be `0x504B0102`.
  * The end of central directory header magic number is `0x57515701` but should be `0x504B0506`.
* All (two) filenames in these headers had their bytes `xor 0xE5`'d.

Changing the things above gives you a standard ZIP file. At least 7-Zip is already fine if you just fix the local block header magic number, even though the file will have a strange filename then.

| Emulator              | Version   | Git Commit | File Extensions in `libretro`  |
| --------------------- | --------- | ---------- | ---------------- |
| **FCEUmm**            | _none_    | `7cdfc7e`  | `.fds`, `.nes`, `.unf`, ~~`.unif`~~ |
| **Mednafen PCE Fast** | v0.9.38.7 | _unknown_  | `.pce`, ~~`.cue`~~, ~~`.ccd`~~, ~~`.chd`~~ |
| **Snes9x 2005**       | v1.36     | _unknown_  | `.smc`, `.fig`, `.sfc`, `.gd3`, `.gd7`, `.dx2`, `.bsx`, `.swc` |
| **PicoDrive**         | 1.91      | `cbc93b6`  | `.bin`, `.gen`, `.smd`, `.md`, ~~`.32x`~~, ~~`.cue`~~, ~~`.iso`~~, `.sms`  |
| **TGB Dual**          | v0.8.3    | `9be31d3`  | `.gb`, `.gbc`, `.sgb` |
| **gpSP**              | v0.91     | `261b2db`  | `.gba`, ~~`.bin`~~, `.agb`, `.gbz` |

Extensions the GB300 does not even display are stroke-out. `.bin` files are associated with PicoDrive, not gpSP.
* `.bkp`
* `.zip`
* `.zfc`
* `.zpc`
* `.zmd`
* `.zgb`
* `.zfb` ("link" with thumbnail, intended for arcade games)
* `.nfc` (Famicon, often used for stock ROMs)

There are no signs of other supported emulators, but it looks like MPEG-2 support is included but inaccessible. If you force the GB300 to display `.chd` files, opening one will cause it to load indefinitely. Same goes for `.cue` files no matter if MD-CD or PCE-CD.

### Nintendo Entertainment System

For some reason, _all_ (unpacked) FC ROMs had their header changed. `0x07` to `0x0F` (counting from 0) are zeroed. Sometimes `0x06` is changed as well (but not zeroed).

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


### PC Engine

Performance of PCE is really good. Even the "3D" racing games run very smoothly.

There seems to be no way to get it to run TurboGrafx-CD games.


### Super Nintendo Entertainment System

With the arcade removed, the SNES is the only other system where games can have a bad performance. However, the SNES a lot less issues than the GBA.

Final Knockout does work on the GB300. It was broken on the SF2000 because the bytes from `0xA8000` to `0xBFFFF` of the .zsf file were replaced with unknown data.

Compared to the SF2000, the following file is missing:
* `手柄测试.zsf`


### SEGA Mega Drive and SEGA Master System

Only MD is advertised and there are no SMS games included. The device will still play them if you add them yourself. The button assignments are strange, though.

Despite the undocumented support for SMS, you aren't that lucky with the other Sega consoles here:
* Sega CD games (`.bin` or `.cue` with changed extension or by forcing the GB300 to show them) will load indefinitely/freeze the console, even the tiniest and/or single-image ones. You must change the extension to a supported one to try `.cue` files.
* Most Sega 32X games will not display anything at all, even if you put the correct BIOS in the root or Roms directory. A few games like Knuckles' Chaotix display an error message. You must change the extension to a supported one to try this.
* Many Game Gear games will load. (The games that don't load will have a black screen.) Colors and sometimes graphics outside the Game Gears center 160x144 area are severely glitched, but Audio is fine. If you don't mind the weird colors, you could play, if it wasn't that most buttons aren't mapped. There seem to be only two working buttons that the GB300 calls B and C. By default, they are mapped to its physical B and R buttons respectively. None of these (nor the direction, start and select buttons) correspond to the start button, so you can't start a game. You must change the extension to a supported one to try this.
* Most SG-1000 games will load. (The games that don't load will freeze the device, e.g. Champion Baseball.) There is no video (black screen), but audio is fine. Buttons are also fine. You must change the extension to a supported one to try this. (Note that apps like Home Basic likely doesn't have sound, so you can't tell if it's loading or not.)

Compared to the SF2000, the following game is missing:
* `007 Shitou - The Duel.zmd`

But one thing got better: Instead of the 225 broken thumbnails on the SF2000, there are only 45 on the GB300. These thumbnails were saved in RGBA8888 instead of RGB565. However, the dimension is correct. Despite the thumbnail taking up twice the space, the device is still able to find the archive and run the game. The following MD games have no working thumbnail on the GB300:
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

The GB300 ships with the official (pirated) `gba_bios.bin` in the `bios` folder. This is, however, not the folder where the emulator will look for it. To use the official BIOS, copy it to `\GBA\mnt\sda1\bios\gba_bios.bin` and `\Roms\mnt\sda1\bios\gba_bios.bin` (create all of these folders if they do not exist). Thanks to `bnister` (osaka) for finding this out. One game that requires this procedure is _The Legend of Zelda - The Minish Cap_ (for the main menu), which however does not ship with the device. There are still games that don't work even with that BIOS. The BIOS does not seem to affect the performance. States with and without the BIOS are incompatible. Loading a non-BIOS state when BIOS is active displays the GBA's boot animation and then starts the game. The battery from the state will not be present either.

As in the SF2000, performance varies heavily between games. And even language versions: Probably the oddest example here are the two Advance Wars games, considered the best games for the GBA according to MobyGames. Graphically, they are very simple games. The American version of Advance Wars 2 (a hack of which with Chinese menus ships with the console) is somewhat playable. The American version of Advance Wars 1 works a tiny bit worse but is still playable. The European version of Advance Wars 1 (included with the console) performs too bad to be fun to play. The European Advance Wars 2 is basically unplayable because it's too slow. There is no PAL or NTSC version of the GBA or its games. They're always supposed to run at 60 fps. Using TV output doesn't improve the performance either, no matter if PAL or NTSC. Performance on all Advance Wars games gets even worse when there is any dialogue on screen.

3D games on the GBA don't work well either. Of the five Need for Speed games for example (none of which are included), only Porsche (both regions) and Underground 2 will get past the language selection and the EA logo.


## Tools

Most tools designed for the SF2000 don't work. Tools are often incompatible because not only is the BIOS different, but also the `Resources` have different names. This is especially true for Tadpole. Just starting it already patches your ROM lists and will break all default ROMs. It will only leave the GBA (because the files used for the GBA on the GB300 are used for the arcade on the SF2000, but there is no `ARCADE` folder for Tadpole to scan). If you did use Tadpole, look for the files in the Resources folder with the current date and restore the backups Tadpole put there.

The following tools were made specifically for the GB300:
* [Customized _Frogtool_ (Beta)](https://discord.com/channels/741895796315914271/1195581037003165796/1211025634680119327) by tzlion (original version) and Dteyn (GB300 patch), used for rebuilding the console-dependent ROM lists.
* [GB300 Boot Logo Changer](https://dteyn.github.io/sf2000/tools/bootLogoChangerGB300.htm) by Dteyn

Tools for the SF2000 that should work for the GB300:
* [BIOS CRC-32 Patcher](https://vonmillhausen.github.io/sf2000/tools/biosCRC32Patcher.htm) by Von Millhausen
* [Generic Image Tool](https://vonmillhausen.github.io/sf2000/tools/genericImageTool.htm) by Von Millhausen, to convert to and from RGB565 and BGRA8888 images
* [Kerokero - SF2000 BGM Tool](https://github.com/Dteyn/SF2000_BGM_Tool) by Dteyn
* [Save State Tool](https://vonmillhausen.github.io/sf2000/tools/saveStateTool.htm) by Von Millhausen
* [Silent menu music](https://vonmillhausen.github.io/sf2000/sounds/silentMusic/pagefile.sys) by Von Millhausen
* [Silent Sounds Pack](https://github.com/Dteyn/sf2000/raw/main/sounds/silentSounds/SF2000_Silent_Sounds_Pack.zip) by Dteyn

Other links:
* [Retro Handhelds Discord](https://discord.gg/retrohandhelds), select Data Frog SF2000 during onboarding and join the `#data_frog_sf2000` channel
  * [`Gb300 dev` thread](https://discord.com/channels/741895796315914271/1195581037003165796) 
  * [`GB300 screen swap` thread](https://discord.com/channels/741895796315914271/1197607372277940314) on Retro Handhelds Discord
* [SF2000 Community Compatibility list](https://docs.google.com/spreadsheets/d/19TCedWEKFXlnS2dlmLxk1BcnlHrX-MSVrKwEURuiU0E/edit#gid=1327539659)


## List of ROMs

For the FC, MD, GB, GBC, GBA and SFC see [vonmillhausen's list](https://vonmillhausen.github.io/sf2000/defaultRoms/defaultRomsNoIntroCheck.htm). Games that are missing in the GB300 are listed above in this document. There are no new games for consoles that the SF2000 has, but [Final Knockout](https://datomatic.no-intro.org/index.php?page=show_record&s=49&n=0816) does work on the GB300.

Note that all FC hashes are different for the GB300 because of the reasons explained above.

To play your own games, create the folder `Roms` on the TF card. You can also use ZIP files to save memory.


### List of PCE Games

The following is a list of all PCE games that ship with the GB300. The names listed here are the names on No-Intro. Their catalog knows _all_ of the hashes, meaning that the PCE games are likely no strange hacks like many of the other ROMs. Thumbnails use only covers here, whereas other consoles use at least some screenshots instead. If you remove everything in brackets (and trailing spaces resulting hereof) from the No-Intro name, you get the name on the GB300, with four exceptions given in non-bold brackets after the No-Intro name.

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


## Resources

Note: There are no language strings on the GB300, just a few images.


### Fonts

Only one file, `yahei_Arial.ttf`, identical to the SF2000's font file of the same file name.


### Images

Unlike the SF2000, the GB300 supposedly does not have any unused images (not sure about the 'empty battery' screen though). All of these have been renamed on the GB300 compared to the SF2000.

| File         | Comp's   | Dim's    | Description | View |
| ------------ | -------- | -------- | ----------- | ---- |
| `appvc.ikb`  | BGRA8888 | 150x214  | "missing image" image | [view](/images/appvc.ikb.png) |
| `bfrjd.odb`  | RGB565   | 640x280  | language selection, Korean selected (the seventh item) | [view](/images/bfrjd.odb.png) |
| `bisrv.nec`  | RGB565   | 640x480  | pause menu, third entry selected | [view](/images/bisrv.nec.png) |
| `bttlve.kbp` | BGRA8888 | 60x144   | 6 battery states | [view](/images/bttlve.kbp.png) |
| `bxvtb.sby`  | BGRA8888 | 192x224  | "TV SYSTEM" in 7 different languages | [view](/images/bxvtb.sby.png) |
| `c1eac.pal`  | BGRA8888 | 26x22    | checked checkbox, indicating the selected TV standard and language | [view](/images/c1eac.pal.png) |
| `d2d1.hgp`   | RGB565   | 640x480  | pause menu, second entry selected | [view](/images/d2d1.hgp.png) |
| `dism.cef`   | RGB565   | 640x480  | pause menu, first entry selected | [view](/images/dism.cef.png) |
| `dpskc.ctp`  | RGB565   | 384x320  | 4 different selected save states | [view](/images/dpskc.ctp.png) |
| `drivr.ers`  | BGRA8888 | 150x160  | the five words of the pause menu in Arabic | [view](/images/drivr.ers.png) |
| `dsuei.cpl`  | BGRA8888 | 150x160  | the five words of the pause menu in English | [view](/images/dsuei.cpl.png) |
| `dufdr.cwr`  | BGRA8888 | 240x168  | "SETTING" in 7 different languages | [view](/images/dufdr.cwr.png) |
| `dxva2.nec`  | RGB565   | 640x288  | keyboard for search, embossed keys | [view](/images/dxva2.nec.png) |
| `ectte.bke`  | BGRA8888 | 1200x120 | 12 bottom tab items, default state | [view](/images/ectte.bke.png) |
| `eknjo.ofd`  | RGB565   | 640x280  | language selection, Spanish selected (the fifth item) | [view](/images/eknjo.ofd.png) |
| `exaxz.hsp`  | BGRA8888 | 448x768  | logos in the top left | [view](/images/exaxz.hsp.png) |
| `fhshl.skb`  | RGB565   | 640x280  | language selection, English selected (the first item) | [view](/images/fhshl.skb.png) |
| `fixas.ctp`  | BGRA8888 | 150x160  | the five words of the pause menu in Chinese | [view](/images/fixas.ctp.png) |
| `gpapi.bvs`  | RGB565   | 640x480  | pause menu, fifth entry selected | [view](/images/gpapi.bvs.png) |
| `hctml.ers`  | RGB565   | 320x2256 | 6 images of the device with one of the shoulder and ABXY buttons highlighted | [view](/images/hctml.ers.png) |
| `hlink.bvs`  | RGB565   | 640x288  | keyboard for search, keys with a bright frame | [view](/images/hlink.bvs.png) |
| `icuin.cpl`  | BGRA8888 | 150x160  | the five words of the pause menu in Russian | [view](/images/icuin.cpl.png) |
| `igc64.dll`  | BGRA8888 | 217x37   | Yes/No, No highlighted | [view](/images/igc64.dll.png) |
| `irftp.ctp`  | BGRA8888 | 150x160  | the five words of the pause menu in Korean | [view](/images/irftp.ctp.png) |
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
| `qasfc.bel`  | BGRA8888 | 328x224  | "Favorites are full !" in 7 different languages | [view](/images/qasfc.bel.png) |
| `qdbec.ofd`  | BGRA8888 | 240x168  | "DOWNLOAD ROMS" in 7 different languages | [view](/images/qdbec.ofd.png) |
| `qwave.bke`  | BGRA8888 | 150x160  | the five words of the pause menu in Spanish | [view](/images/qwave.bke.png) |
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
| `xajkg.hsp`  | BGRA8888 | 150x160  | the five words of the pause menu in Portuguese | [view](/images/xajkg.hsp.png) |
| `xjebd.clq`  | BGRA8888 | 448x224  | "No games match the keyword!" in 7 different languages | [view](/images/xjebd.clq.png) |
| `zaqrc.olc`  | BGRA8888 | 8x224    | message box background | [view](/images/zaqrc.olc.png) |
| `ztrba.nec`  | RGB565   | 64x320   | key names (single and prefixed with "T" for autofire; also without and with a whitish background) | [view](/images/ztrba.nec.png) |


### ROM Lists

Your custom ROMs are indexed into `tsmfk.tax` when the device boots. The following files contain the names of the stock ROMs:

| Console | File name file | Chinese name file | Pinyin name file |
| ------- | -------------- | ----------------- | ---------------- |
| **FC**  | `rdbui.tax`    | `fhcfg.nec`       | `nethn.bvs`      |
| **PCE** | `urefs.tax`    | `adsnt.nec`       | `xvb6c.bvs`      |
| **SFC** | `scksp.tax`    | `setxa.nec`       | `wmiui.bvs`      |
| **MD**  | `vdsdc.tax`    | `umboa.nec`       | `qdvd6.bvs`      |
| **GB**  | `pnpui.tax`    | `wjere.nec`       | `mgdel.bvs`      |
| **GBC** | `vfnet.tax`    | `htuiw.nec`       | `sppnp.bvs`      |
| **GBA** | `mswb7.tax`    | `msdtc.nec`       | `mfpmp.bvs`      |


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


### Key Mapping

The GB300 uses a larger `KeyMapInfo.kmp` file because it stores 7 key mappings instead of just 6. This makes its file incompatible with the SF2000 key map editor. Another difference to the SF2000 is that this file does not exist by default and is only created once you assign non-standard keys.

After each emulated console's key map (24 bytes), it is repeated instantly (probably for the second player, but 2-player capabilities of the GB300 are unknown). Then comes the next console.

Consoles are encoded in the following order:

| Console/Order | Physical Button Save Order   | Available Values per Physical Button                                         |
| ------------- | ---------------------------- | ---------------------------------------------------------------------------- |
| 1. **FC**     | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B                                                     |
| 2. **PCE**    | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: X, `0x0B00`: Y, `0x0100`: C, `0x0900`: Z |
| 3. **SFC**    | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: X, `0x0B00`: Y, `0x0100`: L, `0x0900`: R |
| 4. **MD/SMS** | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: X, `0x0B00`: Y, `0x0100`: C, `0x0900`: Z |
| 5. **GB/GBC** | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B                                                     |
| 6. **GBA**    | `X`, `Y`, `L`, `A`, `B`, `R` | `0x0800`: A, `0x0000`: B, `0x0A00`: X, `0x0B00`: Y, `0x0100`: L, `0x0900`: R |
| 7. unknown    |                              | defaults identical to SFC's defaults                                         |

After each button's 16-bit value from the table above comes a 16-bit flag for autofire: `0x0100` if autofire is active (indicated by a `T` in the console's key map editor), `0x0000` if not.

Note that not all of the selectable button values actually exist on the console. For example, the GBA does not have X and Y buttons and will treat both like A.

R and L buttons are swapped when you use the console's key map editor. If the button on the left (next to the five menu items where you selected "Joystick") is highlighted, you'll set the button physically labeled R. The table above uses the _physical_ labels (found on the device's case), not what is highlighted by the console's key map editor.

Per-game key mappings do not seem to work.


### Other Files

* `Archive.sys`
* `Favorites.bin`
* `Foldername.ini`
* `History.bin`

Once again, their format and use is identical to the SF2000. `Foldername.ini` however is adjusted to the GB300's lack of arcade and additional PCE feature. Its default content is:

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
