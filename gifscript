#!/bin/bash
echo "1"

i=1
sudo pkill -9 led
for ((i = 2; i <= 50; i++))
do
	echo {"GIF" $i}
	sudo /home/pi/rpi-rgb-led-matrix/utils/led-image-viewer  --led-scan-mode=0 --led-brightness=100 --led-no-hardware-pulse --led-gpio-mapping=adafruit-hat-pwm -f /home/pi/gifs/"$i".gif &
	sleep 25
	sudo pkill -9 led
	wait 1
	if (($i == 49))
	then
		i=1
	fi
done
echo "YEET"
exit