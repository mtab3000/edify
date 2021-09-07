# The Passive Edification Screen - Codename: Audrey

![Action Shot](/images/thequote.png)

This is a script that uses similar smarts to the [crypto ticker](https://github.com/llvllch/btcticker), but to display randomly-chosen highly-rated **quotes** from Reddit (r/quotes), a **word of the day** or a item from a **flashcard** text file (selected at random, according to weightings in config file `config.yaml`)

# Getting started
## Prerequisites

(These instructions assume that your Raspberry Pi is already connected to the Internet, happily running `pip` and has `python3` installed)

If you are running the Pi headless, connect to your Raspberry Pi using `ssh`.

Install the Waveshare Python module following the instructions on their Wiki under the tab Hardware/Software setup.

(To install the waveshare_epd python module, you need to run the setup file in their repository - also, be sure **not** to install Jetson libraries on a Pi)

```#Install BCM2835 libraries
wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.60.tar.gz
tar zxvf bcm2835-1.60.tar.gz 
cd bcm2835-1.60/
sudo ./configure
sudo make
sudo make check
sudo make install
#For more details, please refer to http://www.airspayce.com/mikem/bcm2835/```

```#Install wiringPi libraries
sudo apt-get install wiringpi```

#For Pi 4, you need to update it：
cd /tmp
wget https://project-downloads.drogon.net/wiringpi-latest.deb
sudo dpkg -i wiringpi-latest.deb
gpio -v
#You will get 2.52 information if you install it correctly```

```#Install Python libraries

##python2
#sudo apt-get update
#sudo apt-get install python-pip
#sudo apt-get install python-pil
#sudo apt-get install python-numpy
#sudo pip install RPi.GPIO
#sudo pip install spidev

#python3
sudo apt-get update
sudo apt-get install python3-pip
sudo apt-get install python3-pil
sudo apt-get install python3-numpy
sudo pip3 install RPi.GPIO
sudo pip3 install spidev```

```#git
sudo apt-get install git -y```

```#clone repo
sudo git clone https://github.com/waveshare/e-Paper
```

```
cd e-Paper/RaspberryPi_JetsonNano/python
sudo python3 setup.py install
```

## Install & Run

Copy the files from this repository onto the Pi, or clone using:

```cd ~
git clone https://github.com/llvllch/edify.git
cd edify
```

Install the required modules using pip:

```python3 -m pip install -r requirements.txt```

If you'd like the script to persist once you close the session, use screen.

Start a screen session:

```screen bash```

Run the script using:

```python3 edify.py```

Detatch from the screen session using CTRL-A followed by CTRL-D

The unit will now pull data every 60 minutes and update the display.

## Links

You can buy a fully assembled Audrey or frames at [veeb.ch](https://www.veeb.ch/store/p/neverending-quotes)

# Service
```
cat <<EOF | sudo tee /etc/systemd/system/edify.service
[Unit]
Description=edify
After=network.target

[Service]
ExecStart=/usr/bin/python3 -u /home/pi/edify/edify.py
WorkingDirectory=/home/pi/edify/
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOF
```

# Troubleshooting

Some people have had errors on a clean install of Rasbian Lite on Pi. If you do, run:

```
sudo apt-get install libopenjp2-7
sudo apt-get install libqt5gui5
sudo apt-get install python-scipy
sudo apt install libatlas-base-dev
```

and re-run the script.

If the unit is freezing, try switching to another power supply. I've lost count of the number of times I've spent half a day trying to debug what turned out to be a dodgy power supply.

# Licencing

GNU GENERAL PUBLIC LICENSE Version 3.0
