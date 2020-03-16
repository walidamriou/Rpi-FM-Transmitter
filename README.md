# Raspberry pi as FM transmitter 
Use the Raspberry Pi as an FM transmitter. Works on every Raspberry Pi board.

Just get an FM receiver, connect a 20 - 40 cm plain wire to the Raspberry Pi's GPIO4 (PIN 7 on GPIO header) to act as an antena, and you are ready for broadcasting.

This project uses the general clock output to produce frequency modulated radio communication. It is based on an idea originally presented by [Oliver Mattos and Oskar Weigl](http://icrobotics.co.uk/wiki/index.php/Turning_the_Raspberry_Pi_Into_an_FM_Transmitter) at [PiFM project](http://icrobotics.co.uk/wiki/index.php/Turning_the_Raspberry_Pi_Into_an_FM_Transmitter).
## How to use it
To use this project you will have to build the executable. First, clone this repository, then use `make` command as shown below:
```
git clone https://github.com/walidamriou/Rpi-FM-Transmitter
cd fm_transmitter
make
``` 
After a successful build you can start transmitting by executing the "fm_transmitter" program, in this example I will transmits at 93.5 Mhz:
```
sudo ./fm_transmitter -f 93.5 acoustic_guitar_duet.wav
```
Where:
* -f frequency - Specifies the frequency in MHz, 100.0 by default if not passed
* acoustic_guitar_duet.wav - Sample WAV file, you can use your own

Other options:
* -d dma_channel - Specifies the DMA channel to be used (0 by default), type 255 to disable DMA transfer, CPU will be used instead
* -b bandwidth - Specifies the bandwidth in kHz, 100 by default
* -r - Loops the playback

After transmission has begun, simply tune an FM receiver to chosen frequency, You should hear the playback.
### Supported audio formats
You can transmitt uncompressed WAV (.wav) files directly or read audio data from stdin, eg.:
```
sudo apt-get install sox
sox star_wars.wav -r 22050 -c 1 -b 16 -t wav - | sudo ./fm_transmitter -f 100.6 -
```
Please note only uncompressed WAV files are supported. If you receive the "corrupted data" error try converting the file, eg. by using SoX:
```
sudo apt-get install sox libsox-fmt-mp3
sox my-audio.mp3 -r 22050 -c 1 -b 16 -t wav my-converted-audio.wav
sudo ./fm_transmitter -f 100.6 my-converted-audio.wav
```
### Microphone support
In order to use a microphone live input use the `arecord` command, eg.:
```
arecord -D hw:1,0 -c1 -d 0 -r 22050 -f S16_LE | sudo ./fm_transmitter -f 100.6 -
```
In cases of a performance drop down use ```plughw:1,0``` instead of ```hw:1,0``` like this:
```
arecord -D plughw:1,0 -c1 -d 0 -r 22050 -f S16_LE | sudo ./fm_transmitter -f 100.6 -
```
## Legal note
Please keep in mind that transmitting on certain frequencies without special permissions may be illegal in your country.
## New features
* DMA peripheral support
* Allows custom frequency and bandwidth settings
* Works on every Raspberry Pi model
* Reads mono and stereo files
* Reads data from stdin

## Tested
I've test this project with Raspberry pi 4 model B and it worked perfectly.  

---------------------------------------------------------------------------------------------

Included sample audio was created by [graham_makes](https://freesound.org/people/graham_makes/sounds/449409/) and published on [freesound.org](https://freesound.org/)
