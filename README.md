[![build status image](https://travis-ci.org/neurobin/shc.svg?branch=release)](https://travis-ci.org/neurobin/shc)
[![GitHub stars](https://img.shields.io/github/stars/neurobin/shc.svg)](https://github.com/neurobin/shc/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/neurobin/shc.svg)](https://github.com/neurobin/shc/network)
[![GitHub issues](https://img.shields.io/github/issues/neurobin/shc.svg)](https://github.com/neurobin/shc/issues)

# Shell Script Compiler

A generic shell script compiler. Shc takes a script, which is specified on the command line and produces C source code. The generated source code is then compiled and linked to produce a stripped binary executable.

The compiled binary will still be dependent on the shell specified in the first line of the shell code (i.e shebang) (i.e. #!/bin/sh), thus shc does not create completely independent binaries.

shc itself is not a compiler such as cc, it rather encodes and encrypts a shell script and generates C source code with the added expiration capability. It then uses the system compiler to compile a stripped binary which behaves exactly like the original script. Upon execution, the compiled binary will decrypt and execute the code with the shell -c option.

## Install

1. ./configure
2. make
3. sudo make install

**Note** If `make` fails due to *automake* version, run `./autogen.sh` before running the above commands.

### Ubuntu-specific

```
sudo add-apt-repository ppa:neurobin/ppa
sudo apt-get update
sudo apt-get install shc
```

If the above installation method seems like too much work, then just download a compiled binary package from [release page](https://github.com/neurobin/shc/releases/latest) and copy the `shc` binary to `/usr/bin` and `shc.1` file to `/usr/share/man/man1`.

## Usage

```
shc [options]
shc -f script.sh -o binary
shc -U -f script.sh -o binary # Untraceable binary (prevent strace, ptrace etc..)
shc -H -f script.sh -o binary # Untraceable binary, does not require root (only bourne shell (sh) scripts with no parameter)
shc -H -s -f script.sh -o binary # Untraceable binary running in a singe process, does not require root (only bourne shell (sh) scripts with no parameter)
```

## Testing

```bash
./configure
make
make check
```

## Known bugs

The one (and I hope the only) limitation using shc is the _SC_ARG_MAX system configuration parameter.
It limits the maximum length of the arguments to the exec function, limiting the maximum length of the runnable script of shc.

!! - CHECK YOUR RESULTS CAREFULLY BEFORE USING - !!

## Links

1. [Man Page](http://neurobin.github.io/shc/man.html)
2. [Web Page](http://neurobin.github.io/shc)

# Contributing

If you want to make pull requests, please do so against the **master** branch. The default branch is **release** which should contain clean package files ready to be used.

If you want to edit the manual, please edit the **man.md** file (available in the master branch) instead and then generate the manual file from it with the command (requires `pandoc` to be installed):

```bash
pandoc -s man.md -t man -o shc.1
#also run this command to generate the html manual
pandoc -s man.md -t html -o man.html
```

If you change anything related to autotools, please run `./autogen.sh` afterwards.
