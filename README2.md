# Aptdec 1.7

## ATPDEC README (Thierry Leconte F4DWV (c) 2004-2009)

Aptdec - Create images from weather satellites transmissions. NOAA satellites transmit [Automatic Picture Transmissions (APT) wiki](https://en.wikipedia.org/wiki/Automatic_picture_transmission).
Decode and translate Automatic Picture Transmissions (APT) from WAV audio files to images.

- [INSTALLATION](##installation)
- [USAGE](##usage)
- [DESCRIPTION](##description)
- [OPTIONS](##options)
- [OUTPUT](##output)
- [EXAMPLE](##example)

Atpdec is an open source program that decodes images transmitted by POES 
NOAA weather satellite series.
These satellites transmit continuously, among other things, medium 
resolution images of the earth on 137Mhz.
These transmissions could be easily received with an inexpensive antenna 
and dedicated receiver.
Output from such a receiver, is an audio signal that could be recorded 
into a soundfile with any soundcard.

Atpdec will convert these sounfiles into .png images.

For each soundfile up to 6 images could be generated :

1. Raw image : contains the 2 transmitted channel images + telemetry and synchro pulses.
2. Calibrated channel A image
3. Calibrated channel B image
4. Temperature compensed I.R image
5. False color image

Input soundfiles must be mono signal sampled at 11025 Hz.
Atpdec use libsndfile to read soundfile, so any sound file format supported by libsndfile
could be read.(Only tested with .wav file).

## INSTALLATION

Atpdec is written in plain standart C and must be very portable.
It was only tested on Linux Fedora , but must work on any Unix platform.
Just adapt the Makefile and type make (sorry no configure). 

Atpdec use libsndfile, libpng and libm.
snd.h and png.h header must be present on your system.
If they are not on standard path, edit the include path in the Makefile.

This is how to install the 1.7 release; its newer than the repo’s master branch. 
Compiling the repo’s master branch without success, 

To install for fedora users (Linux), type:
    
    cd ~
    wget https://github.com/csete/aptdec/archive/v1.7.tar.gz
    tar -xvf aptdec-1.7.tar.gz
    sudo dnf install openlibm.x86_64
    sudo dnf install libpng-devel.x86_64
    sudo dnf install libsndfile-devel.x86_64
    cd aptdec-1.7
    make
    
## USAGE
atpdec [options] soundfiles ...

    cd ~/aptdec-1.7 && ./atpdec
    
## DESCRIPTION
**youtube-dl** is a command-line program to download videos from YouTube.com and a few more sites. It requires the Python interpreter, version 2.6, 2.7, or 3.2+, and it is not platform specific. It should work on your Unix box, on Windows or on macOS. It is released to the public domain, which means you can modify it, redistribute it or use it however you like.

    youtube-dl [OPTIONS] URL [URL...]

## OPTIONS

    -i [r|a|b|c|t]
	   Toggle raw (r) , channel A (a) , channel B (b) , false color (c) ,
    or temperature (t) output.
    Default : "ac"

    -d directory
    Optional images destination directory.
    Default : soundfile directory.

    -s n
    Satellite number 15 to 19 
    Used for Temperature compensation.
    Default :  NOAA-19

    -c conf_file
    Use configuration file for false color generation.
    Default : Internal parameters.

## OUTPUT

Generated image are in png format, 8bits greyscale for raw and channel A|B images,
24bits RVB for false color.

Image names are soundfilename-x.png, where x is :
    -r for raw images
    -satellite instrument number (1,2,3A,3B,4,5) for channel A|B images
    -c for false colors.

## EXAMPLE

```bash
./atpdec -d image -i ac *.wav
```
Will process all .wav files in the current directory, generate only channel A and false color images and put them in the image directory.
