SDL2 binding for Go [![Build Status](https://travis-ci.org/veandco/go-sdl2.svg?branch=master)](https://travis-ci.org/veandco/go-sdl2)
===================
go-sdl2 is SDL2 wrapped for Go users. It enables interoperability between Go and the SDL2 library which is written in C. That means the original SDL2 installation is required for this to work.

Requirements
============
* [SDL2](http://libsdl.org/download-2.0.php)
* [SDL2_mixer (optional)](http://www.libsdl.org/projects/SDL_mixer/)
* [SDL2_image (optional)](http://www.libsdl.org/projects/SDL_image/)
* [SDL2_ttf (optional)](http://www.libsdl.org/projects/SDL_ttf/)
* [SDL2_gfx (optional)](http://www.ferzkopp.net/wordpress/2016/01/02/sdl_gfx-sdl2_gfx/)

Below is some commands that can be used to install the required packages in
some Linux distributions. Some older versions of the distributions such as
Ubuntu 13.10 may also be used but it may miss an optional package such as
_libsdl2-ttf-dev_ on Ubuntu 13.10's case which is available in Ubuntu 14.04.

On __Ubuntu 14.04 and above__, type:  
`apt install libsdl2{,-mixer,-image,-ttf,-gfx}-dev`

On __Fedora 25 and above__, type:  
`yum install SDL2{,_mixer,_image,_ttf,_gfx}-devel`

On __Arch Linux__, type:  
`pacman -S sdl2{,_mixer,_image,_ttf,_gfx}`

On __Gentoo__, type:  
`emerge -av libsdl2 sdl2-{gfx,image,mixer,ttf}`

On __Mac OS X__, install SDL2 via [Homebrew](http://brew.sh) like so:  
`brew install sdl2{,_image,_ttf,_mixer} pkg-config`

On __Windows__,  
1. Install mingw-w64 from [Mingw-builds](http://mingw-w64.org/doku.php/download/mingw-builds)  
        - Version: latest (at time of writing 6.3.0)  
        - Architecture: x86_64  
        - Threads: win32  
        - Exception: seh  
        - Build revision: 1  
        - Destination Folder: Select a folder that your Windows user owns  
2. Install SDL2 http://libsdl.org/download-2.0.php  
        - Extract the SDL2 folder from the archive using a tool like [7zip](http://7-zip.org)  
        - Inside the folder, copy the `i686-w64-mingw32` and/or `x86_64-w64-mingw32` depending on the architecture you chose into your mingw-w64 folder e.g. `C:\Program Files\mingw-w64\x86_64-6.3.0-win32-seh-rt_v5-rev1\mingw64`  
3. Setup Path environment variable  
        - Put your mingw-w64 binaries location into your system Path environment variable. e.g. `C:\Program Files\mingw-w64\x86_64-6.3.0-win32-seh-rt_v5-rev1\mingw64\bin` and `C:\Program Files\mingw-w64\x86_64-6.3.0-win32-seh-rt_v5-rev1\mingw64\x86_64-w64-mingw32\bin`  
4. Open up a terminal such as `Git Bash` and run `go get -v github.com/veandco/go-sdl2/sdl`. To prove that it's working correctly, you can change directory by running `cd go/src/github.com/veandco/go-sdl2/examples/events` and run `go run events.go`. A window should pop up and you can see event logs printed when moving your mouse over it or typing on your keyboard.  
5. (Optional) You can repeat __Step 2__ for [SDL_image](https://www.libsdl.org/projects/SDL_image), [SDL_mixer](https://www.libsdl.org/projects/SDL_mixer), [SDL_ttf](https://www.libsdl.org/projects/SDL_ttf)  
        - NOTE: pre-build the libraries for faster compilation by running `go install github.com/veandco/go-sdl2/{sdl,img,mix,ttf}`  

or you can install SDL2 via [Msys2](https://msys2.github.io) like so:
`pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-SDL2{,_mixer,_image,_ttf}`


Installation
============
To get the bindings, type:  
`go get -v github.com/veandco/go-sdl2/sdl`  
`go get -v github.com/veandco/go-sdl2/mix`  
`go get -v github.com/veandco/go-sdl2/img`  
`go get -v github.com/veandco/go-sdl2/ttf`

or type this if you use Bash terminal:  
`go get -v github.com/veandco/go-sdl2/{sdl,mix,img,ttf}`

Due to `go-sdl2` being under active development, a lot of breaking changes are going to happen during v0.x. Therefore if you want to stay with the latest stable version, you should replace `github.com/veandco/go-sdl2` with `gopkg.in/veandco/go-sdl2.v0` so it will refer to the latest stable version e.g. `gopkg.in/veandco/go-sdl2.v0/sdl`.

__Note__: If you didn't use the previous commands or use 'go install', you will experience long
compilation time because Go doesn't keep the built binaries unless you install them.

Example
=======
```go
package main

import "github.com/veandco/go-sdl2/sdl"

func main() {
	if err := sdl.Init(sdl.INIT_EVERYTHING); err != nil {
		panic(err)
	}
	defer sdl.Quit()

	window, err := sdl.CreateWindow("test", sdl.WINDOWPOS_UNDEFINED, sdl.WINDOWPOS_UNDEFINED,
		800, 600, sdl.WINDOW_SHOWN)
	if err != nil {
		panic(err)
	}
	defer window.Destroy()

	surface, err := window.GetSurface()
	if err != nil {
		panic(err)
	}

	rect := sdl.Rect{0, 0, 200, 200}
	surface.FillRect(&rect, 0xffff0000)
	window.UpdateSurface()

	sdl.Delay(2500)
}
```

For more complete examples, see inside the _examples_ folder. Run any of the .go files with `go run`.


Cross-compiling
===============

### Linux to Windows

1. Install MinGW toolchain.
   * On **Arch Linux**, it's simply `pacman -S mingw-w64`.
2. Download the SDL2 development package for MinGW [here](http://libsdl.org/download-2.0.php) (and the others like *SDL_image*, *SDL_mixer*, etc.. [here](https://www.libsdl.org/projects/) if you use them).
3. Extract the SDL2 development package and copy the `x86_64-w64-mingw32` folder inside recursively to the system's MinGW `x86_64-w64-mingw32` folder. You may also do the same for the `i686-w64-mingw32` folder.
   * On **Arch Linux**, it's `cp -r x86_64-w64-mingw32 /usr`.
4. Now you can start cross-compiling your Go program by running `env CGO_ENABLED="1" CC="/usr/bin/x86_64-w64-mingw32-gcc" GOOS="windows" CGO_LDFLAGS="-lmingw32 -lSDL2" CGO_CFLAGS="-D_REENTRANT" go build -x main.go`. You can change some of the parameters if you'd like to. In this example, it should produce a `main.exe` executable file.
5. Before running the program, you need to put `SDL2.dll` from the [SDL2 runtime package](http://libsdl.org/download-2.0.php) (For others like *SDL_image*, *SDL_mixer*, etc.., look for them [here](https://www.libsdl.org/projects/)) for Windows in the same folder as your executable.
6. Now you should be able to run the program using Wine or Windows!

FAQ
===
__Why does the program not run on Windows?__  
Try putting the [runtime libraries](http://libsdl.org/download-2.0.php) (e.g. `SDL2.dll` and friends) in the same folder as your program.

__Why does my program crash randomly or hang?__  
Putting `runtime.LockOSThread()` at the start of your main() usually solves the problem (see [SDL2 FAQ](https://wiki.libsdl.org/FAQDevelopment) about multi-threading).

UPDATE: Recent update added a call queue system where you can put thread-sensitive code and have it called synchronously on the same OS thread. See the `render_queue` or `render_goroutines` examples to see how it works.

__Why can't SDL_mixer seem to play MP3 audio file?__  
Your installed SDL_mixer probably doesn't support MP3 file.

On __Mac OS X__, this is easy to correct. First remove the faulty mixer: `brew remove sdl2_mixer`, then reinstall it with the MP3 option: `brew install sdl2_mixer --with-flac --with-fluid-synth --with-libmikmod --with-libmodplug --with-smpeg2`. If necessary, check which options you can enable with `brew info sdl2_mixer`.

On __Other Operating Systems__, you will need to compile smpeg and SDL_mixer from source with the MP3 option enabled. You can find smpeg in the `external` directory of SDL_mixer. Refer to issue [#148](https://github.com/veandco/go-sdl2/issues/148) for instructions.

__Does go-sdl2 support compiling on mobile platforms like Android and iOS?__  
For Android, see https://github.com/gen2brain/go-sdl2-android-example.

There is currently no support for iOS yet.

License
=======
Go-SDL2 is BSD 3-clause licensed.
