# FreeRTOS Emulator using Docker Containers

WORK IN PROGRESS!

## Prerequisites

The approach for the emulator is based on Docker-Desktop and X11 which is heavily OS-dependent. To make the usage more generic a Makefile (See `docker/Makefile`) is available. The the folowing sections the initial required steps for setting up the development environment

### Debian/Ubuntu

TODO
```bash

```

### ARCH

TODO

### Windows

- Install Docker-Desktop (https://hub.docker.com/editions/community/docker-ce-desktop-windows)
- Enable Hyper-V Feature in Windows
Open Powershell with Admin Privileges and run:
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```
- Reboot the Computer
- Install VcXsrv Windows X Server (VcXsrv Windows X Server)
- Start `XLaunch` and configure it using the default until you get to Extra Settings where you have to ensure, that "disable access control" is checked. Save the Configurating. The X Window Server should be running now. (Make sure, that software like `Xming` (OLD) is NOT running in parallel)
- 
### MacOS

TODO
```bash
sudo pacman -S sdl2 sdl2_gfx sdl2_image sdl2_mixer sdl2_ttf
```

## Usage

### General

TODO
```bash
cd build
cmake ..
make
```

### Comiling

TODO

### Running

TODO

### Debugging

TODO

## Documentation

Documentation of the crude graphics library, found in `lib/Gfx`, and the example collision demo code library, can be generated by running

``` bash
doxygen
```
from the `docs` directory to generate the Doxygen documentation. Also generated on GitHub via Travis CI, available [*here*](https://alxhoff.github.io/FreeRTOS-Emulator/).

## Asynchronous IO

There are three implemented methods to perform asynchronous IO, with the hope of emulating the sort of IO you find on micro-controllers. The three methods are UDP and TCP sockets and POSIX message queues.

### TCP/UDP Sockets

Both protocols have the same functionality, differing only in that the UDP callback is called for every received transmission whilst the TCP callback is called for every established connection. Both work by establishing a connection to the given IPv4 dot-decimal address and port.

### POSIX Message Queues

Similarly to UDP sockets, message queues trigger their callback once for each message received. The message queue names, unlike the Linux POSIX implementation, do not require the prepending '/'.

### Callbacks

Each type of connection can be assigned its own callback function. The callback, if provided (else `NULL`), must take three arguments: `size_t`, `char *`, `void *`. These parameters, respectively, provide the callback with the number of bytes received into the buffer, the buffer and the args given as an argument when the connection was created.

### Sending to Connections

`aIOMessageQueuePut` and `aIOSocketPut` can be used to send a `char *` buffer to either a message queue (specified using the message queue's name) or a socket (specified using an IPv4 dot-decimal address and a port).

## Example

TODO

## YouCompleteMe Integration

This is not really related to the project but as I have left in the YCM symlinks and options in the CMake I may as well detail the YouCompleteMe Vim integration as it is applicable for other projects as well.

### Install vim-plug

#### Ubuntu users

Ubuntu does not yet install vim 8.X+ by default and as you must install it manually first

``` bash
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update
sudo apt install vim
```
#### Installation

##### Prerequisites

A python version >= 3.5 is required.

``` bash
sudo apt install build-essential cmake3 python3.5-dev python3.5
```

##### Vim plugin manager

``` bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

##### YCM

Add YCM to vimrc

``` bash
echo "call plug#begin('~/.vim/plugged')" >> $HOME/.vimrc
echo "Plug 'valloric/youcompleteme'" >> $HOME/.vimrc
echo "call plug#end()" >> $HOME/.vimrc
```

Start vim and run `:PlugInstall`

Navigate to vim plugin folder and run install script

```bash
cd $HOME/.vim/plugged/youcompleteme
python3.5 install.py --clang-completer
```
Get the config script

``` bash
curl -Lo $HOME/.ycm_extra_conf.py https://raw.githubusercontent.com/alxhoff/dotfiles/master/ycm/.ycm_extra_conf.py
```

After running, you should be able to complete using CRTL+Space
