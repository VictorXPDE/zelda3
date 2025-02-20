# Zelda3
A reimplementation of Zelda 3.

Our discord server is: https://discord.gg/AJJbJAzNNJ

## About

This is a reverse engineered clone of Zelda 3 - A Link to the Past.

It's around 70-80kLOC of C code, and reimplements all parts of the original game. The game is playable from start to end.

You need a copy of the ROM to extract game resources (levels, images). Then once that's done, the ROM is no longer needed.

It uses the PPU and DSP implementation from [LakeSnes](https://github.com/elzo-d/LakeSnes), but with lots of speed optimizations.
Additionally, it can be configured to also run the original machine code side by side. Then the RAM state is compared after each frame, to verify that the C implementation is correct.

I got much assistance from spannerism's Zelda 3 JP disassembly and the other ones that documented loads of function names and variables.

## Additional features

Some features have been added that are not supported by the original game.

Secondary item slot on button X (Hold X in inventory to select).

Displays max rupees, bombs and arrows with yellow or orange color.

Extends throwing bombs to four instead of two.

Support for MSU audio tracks.

Support for enhanced aspect ratios of 16:9 or 16:10.

Switching current item with L/R keys.

Reordering of inventory by pressing Y+Arrows.

Higher quality map screen.

Disable low health beep.

Pick up items and destroy pots with Sword.

## Installing Python & libraries on Windows (required for asset extraction steps)
1. Download [Python](https://www.python.org/ftp/python/3.10.7/python-3.10.7-amd64.exe) installer and install
2. Open the command prompt
3. Upgrade pip & install `pillow` and `pyyaml` by typing `python -m pip install --upgrade pip pillow pyyaml` and hit enter
4. Close the command prompt

## Compiling on Windows with TCC (1mb Tiny C Compiler)
1. Download the project by clicking "Code > Download ZIP" on the github page
2. Extract the ZIP to your hard drive
3. Place the USA rom named `zelda3.sfc` in the "\tables" subfolder
4. Open the command prompt and navigate to that folder
5. Extract resources by typing `python extract_resources.py` and hit enter
6. Compile the extracted resources by typing `python compile_resources.py` and hit enter
7. Close the command prompt
8. Download [TCC](https://download.savannah.gnu.org/releases/tinycc/tcc-0.9.27-win64-bin.zip) and extract to the "\third_party" subfolder
9. Download [SDL2](https://github.com/libsdl-org/SDL/releases/download/release-2.24.0/SDL2-devel-2.24.0-VC.zip) and extract to the "\third_party" subfolder
10. Double-click `run_with_tcc.bat` in the main dir to create `zelda3.exe` in that same dir
11. Configure with `zelda3.ini` in the main dir

## Compiling on Windows with Visual Studio (4.5gb IDE and compiler)
Same Steps 1-7 above<br/>
8. Double-click `Zelda3.sln`<br/>
9. Change "debug" to "release" in the top dropdown<br/>
10. Choose "build > build Zelda3" in the menu to create `zelda3.exe` in the "/bin/release" subfolder<br/>
11. Configure with `zelda3.ini` in the main dir<br/>

## Installing libraries on Linux/MacOS
1. Open a terminal
2. Install pip if not already installed
```sh
python3 -m ensurepip
```
3. Clone the repo and `cd` into it
```sh
git clone https://github.com/snesrev/zelda3
cd zelda3
```
4. Install requirements using pip
```sh
python3 -m pip install -r requirements.txt
```
5. Install SDL2
* Ubuntu/Debian `sudo apt install libsdl2-dev`
* Fedora Linux `sudo dnf in sdl2-devel`
* Arch Linux `sudo pacman -S sdl2`
* macOS: `brew install sdl2` (you can get homebrew [here](https://brew.sh/))

## Compiling on Linux/MacOS
1. Place your US ROM file named `zelda3.sfc` in `zelda3/tables`
2. Compile
```sh
make
```
<details>
<summary>
Advanced make usage ...
</summary>

```sh
make -j$(nproc) # run on all core
make clean all  # clear gen+obj and rebuild
CC=clang make   # specify compiler
```
</details>

## Nintendo Switch

You need [DevKitPro](https://devkitpro.org/wiki/Getting_Started) and [Atmosphere](https://github.com/Atmosphere-NX/Atmosphere) installed.

```sh
(dkp-)pacman -S git switch-dev switch-sdl2 switch-tools
cd platform/switch
make # Add -j$(nproc) to build using all cores ( Optional )
# You can test the build directly onto the switch ( Optional )
nxlink -s zelda3.nro
```

## More Compilation Help

Look at the wiki at https://github.com/snesrev/zelda3/wiki for more help.

The ROM needs to be named `zelda3.sfc` and has to be from the US region with this exact SHA256 hash
`66871d66be19ad2c34c927d6b14cd8eb6fc3181965b6e517cb361f7316009cfb`

In case you're planning to move the executable to a different location, please include the file `tables/zelda3_assets.dat`.

## Usage and controls

The game supports snapshots. The joypad input history is also saved in the snapshot. It's thus possible to replay a playthrough in turbo mode to verify that the game behaves correctly.

The game is run with `./zelda3` and takes an optional path to the ROM-file, which will verify for each frame that the C code matches the original behavior.

| Button | Key         |
| ------ | ----------- |
| Up     | Up arrow    |
| Down   | Down arrow  |
| Left   | Left arrow  |
| Right  | Right arrow |
| Start  | Enter       |
| Select | Right shift |
| A      | X           |
| B      | Z           |
| X      | S           |
| Y      | A           |
| L      | C           |
| R      | V           |

The keys can be reconfigured in zelda3.ini

Additionally, the following commands are available:

| Key | Action                |
| --- | --------------------- |
| Tab | Turbo mode |
| W   | Fill health/magic     |
| Shift+W   | Fill rupees/bombs/arrows     |
| Ctrl+E | Reset            |
| P   | Pause (with dim)                |
| Shift+P   | Pause (without dim)                |
| Ctrl+Up   | Increase window size                |
| Ctrl+Down   | Decrease window size                |
| T   | Toggle replay turbo mode  |
| O   | Set dungeon key to 1  |
| K   | Clear all input history from the joypad log  |
| L   | Stop replaying a shapshot  |
| R   | Toggle between fast and slow renderer |
| F   | Display renderer performance |
| F1-F10 | Load snapshot      |
| Alt+Enter | Toggle Fullscreen     |
| Shift+F1-F10 | Save snapshot |
| Ctrl+F1-F10 | Replay the snapshot |
| 1-9 | run a dungeons playthrough snapshots |
| Ctrl+1-9 | run a dungeons playthrough in turbo mode |


## License

This project is licensed under the MIT license. See 'LICENSE.txt' for details.
