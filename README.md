# RGBGIFSlideshow
Display a loop of GIFs on a 32x32 RBG matrix using a Pi with [Adafruit RGB Matrix HAT + RTC for Raspberry Pi - Mini Kit](https://www.adafruit.com/product/2345)

![as](https://github.com/DIYCharles/RGBGIFSlideshow/blob/main/pics/gif.gif?raw=true "gif.gif")



Table of contents
=================

<!--ts-->
   * [Prerequisites](#Prerequisites)
   * [How To Use](#How-To-Use)
<!--te-->

Prerequisites
============

```sh
sudo apt-get update
 
sudo apt-get install python2.7-dev python-pillow -y

git clone https://github.com/onebeartoe/rpi-rgb-led-matrix.git
```

Run make for the repo with the Adafruit Hat specified.

```sh
cd rpi-rgb-led-matrix
sudo HARDWARE_DESC=adafruit-hat make install-python
``` 
Test to make sure your RGB pannel is working

```sh
cd ../examples-api-use
make

cd rpi-rgb-led-matrix/examples-api-use 
 
sudo ./demo -D -m 0 --led-no-hardware-pulse --led-gpio-mapping=adafruit-hat
```
The test should display this rotating block

<img src="https://cdn-shop.adafruit.com/970x728/2345-06.jpg" alt="drawing" style="width: 25%;"/>
<br></br>

Build the image viewing utility 
```sh
cd utils/
sudo make led-image-viewer
```
Make directory for gifs to be placed

```sh
sudo mkdir /home/pi/gifs
sudo chown pi:pi /home/pi/gifs/
```

How To Use
============

1. Upload GIFs to /home/pi/gifs/ (easiest way if not using desktop UI is remote through WinSCP)
2. Rename the files to numbers counting up from 1 
   ```
   1.gif
   2.gif
   3.gif
   ```
   <img src="https://github.com/DIYCharles/RGBGIFSlideshow/blob/main/pics/winscpfilenum.JPG?raw=true" style="max-width:50%;" />
3. Upload script gifscript.sh (copy pase if using WINSCP or run ``` sudo nano /home/pi/gifscript.sh ``` and past into the file) 
4. Edit gifscript to reflect the number of GIFs
```sh
#!/bin/bash
echo "1"
i=1
sudo pkill -9 led
for ((i = 1; i <= 50; i++)) # Here I have 49 GIFs in the file
do
	echo {"Welcome" $i}
	sudo /home/pi/rpi-rgb-led-matrix/utils/led-image-viewer  --led-scan-mode=0 --led-brightness=100 --led-no-hardware-pulse --led-gpio-mapping=adafruit-hat-pwm -f /home/pi/gifs/"$i".gif &
	sleep 25 # Here I have the amount of time spent on each GIF
	sudo pkill -9 led
	wait 1
	if (($i == 49))
	then
		i=1
	fi
done
echo "GIF show exited"
exit
```
5. Run so the file is executable. This must be done every time you make changes to ```gifscript```
```sh
sudo chmod 755 gifscript
```
6. Run the script
```sh
sudo /home/pi/gifscript
```

<img src="https://github.com/DIYCharles/RGBGIFSlideshow/blob/main/pics/runningscript.JPG?raw=true" style="max-width:50%;" >



# How to find GIFs

Use google advanced search like this.
You can use higher file sizes like 64x64, 128x128, ect but you'll want to convert them to 32x32 or else it will take forever to load.

<img src="https://github.com/DIYCharles/RGBGIFSlideshow/blob/main/pics/advimgsrch.JPG?raw=true" style="max-width:50%;" >