# Hashlink/C Guide for M1 powered Mac (Heaps.io)

**This is a comprehensive guide on how to build hashlink from source for M1 mac and using it to compile your heaps.io game on windows and mac. This does not run  `.hl`, rather, making a heaps project runnable on M1 mac by exporting it to a c project and using gcc to build / compile it.**
 
### Contents
- ##### [Building Hashlink from source](#1-building-hashlink-from-source)
- ##### [Building HL/C](#2.Building-hl-c)
- ##### [(Optional) Easy Compile and Run](#3.Easy-compile-and-run)
&nbsp;

---
---
---
&nbsp;
## 1. Building Hashlink from source
Before we can use any hashlink related command, we need to build it from source. There's no (not that i know of) easy way to get hashlink other than building it directly from source. So here it goes

- Clone hashlink repo
Go to this link : https://github.com/HaxeFoundation/hashlink and clone the repository to one of your folders, preferably named "hashlink"
- Brew bundle
Make sure  you have [brew](https://brew.sh/) installed, open up your terminal and `cd` to your hashlink folder (e.g `cd Documents/hashlink`), then, run `brew bundle` to install the necessary files
- Modifying makefile
Modify your `Makefile` using your favourite text editor like this:
    - remove `-msse2` and `-mfpmath=sse` from `CFLAGS`
    - add `-I /opt/homebrew/opt/openal-soft/include -I /opt/homebrew/include/SDL2 -I /opt/homebrew/opt/jpeg-turbo/include -I /opt/homebrew/opt/libpng/include -I /opt/homebrew/opt/libvorbis/include -I /opt/homebrew/opt/libogg/include -I /opt/homebrew/opt/mbedtls@2/include -I /opt/homebrew/opt/libuv/include` to your `CFLAGS`
    - Set your `LIBFLAGS` to  `-L /opt/homebrew/opt/openal-soft/lib -L/opt/homebrew/lib -L/opt/homebrew/opt/jpeg-turbo/lib -L/opt/homebrew/opt/libpng/lib -L/opt/homebrew/opt/libvorbis/lib -L/opt/homebrew/opt/libogg/lib -L/opt/homebrew/opt/mbedtls@2/lib -L/opt/homebrew/opt/libuv/lib`
    - Comment out `# HL_DEBUG = include/mdbg/mdbg.o include/mdbg/mach_excServer.o include/mdbg/mach_excUser.o` and `# LIB += ${HL_DEBUG}`
    - If you want, you can download the makefile i used in the asset above, or just go [here]() (Untested)
- Back on your terminal, run `make libhl && make libs`, This will give you: `libhl.dylib`, `fmt.hdll`, `mysql.hdll`, `openal.hdll`, `sdl.hdll`, `ssl.hdll`, `ui.hdll` and `uv.hdll`
- You can also try to use the files from my repository [here]()
- Congratulation, you have finished building hashlink libraries, go to the next step 

&nbsp;

## 2.Building HL/C 
- Go to your heaps project, or [create a new one](https://heaps.io/documentation/hello-hashlink.html)
- Create a new `.hxml` file, in this case i name it `macos_c.hxml`
- In your new `.hxml` file, paste this
    ```hxml
    -cp src
    -lib heaps
    -lib hlsdl
    -hl build/macos/main.c
    -main Main
    ```
- in your project folder, run `haxe macos_c.hxml`, if you encounter a `Error: Library hashlink is not installed` and `Error: Build failed`, That's absolutely normal and is part of the process
- This will create a new folder `build/macos` that is filled with the generated C project
- Copy your `libhl.dylib`, `fmt.hdll`, `mysql.hdll`, `openal.hdll`, `sdl.hdll`, `ssl.hdll`, `ui.hdll` and `uv.hdll` from step 1, and paste it to your new `build/macos` folder
- run `cd build/macos` and after that, run this code
    ```sh
    gcc -O3 -o main main.c -lhl -I[/path/to/hashlink/src] -I. -L. fmt.hdll mysql.hdll sdl.hdll openal.hdll ssl.hdll ui.hdll uv.hdll
    ```
- To test out your game, run `./main` on `build/macos` folder

&nbsp;


## 3. (Optional) Easy Compile and Run
To make the process of compiling and running your game repeatedly easier, i've created a simple script to automate the gcc stuff
- Download [macos_c_compile]() and [macos_c_compile_n_run]() from this repository, and inside you will find 
    ```sh
    #!/bin/bash
    cd build/macos
    gcc -O3 -o main main.c -lhl -I/PATH/TO/HASHLINK/SRC/ -I. -L. fmt.hdll mysql.hdll sdl.hdll openal.hdll ssl.hdll ui.hdll uv.hdll
    ```
- Replace `-I/PATH/TO/HASHLINK/SRC/` with your src folder of the hashlink that you've built in step 1
- Do this for both `macos_c_compile` and `macos_c_compile_n_run`
- Make sure you have pasted all the `.dylib` and `.hdll` in your `build/macos` folder
- To compile your game, run `sh macos_c_compile`
- To compile and run your game, run `sh macos_c_compile_n_run`
