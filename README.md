# Raylib for amiibrOS Project
This repo contains a stripped down and slightly modified raylib forked during
version 2.5.0's development. Included is a src directory and Makefile used to
compile raylib (and a few of its extensions) into a shared library (.so) for
amiibrOS, a minimal linux system running on the Raspberry Pi 3B+.

## Usage
If building amiibrOS, do not run the Makefile in src. Please run the Buildroot
make process instead, which will in turn compile and install the raylib library
to where it needs to be on the target filesystem.

## Credit
Credit to:
* https://github.com/raysan5/raylib
* https://github.com/raysan5/raygui
