# How-to-flash-klipper-firmware-on-BigTreeTech-Octopus-V1.0
Instruction for flashing klipper firmware onto BTT Octopus via DFU mode.

## Setup
### For this guide, you'll need the following: ###

 - BTT Octopus V1.0 (**NOT** BTT Octopus Pro V1.1)
 - Your klipper host computer (usually a RPI).
 - High quality USB Type-A to Type-C cable.
 - Two 0.1" pitch jumper

## Instructions
### Reference
**For flashing firmware:**  
https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master/Octopus%20works%20on%20Voron%20v2.4/Firmware/Klipper  
**For updating firmware:**  
https://docs.vorondesign.com/build/software/installing_mainsail.html  
**For other stuffs:**  
https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master

### Compile firmware
Open PowerShell, SSH into your host:  

    ssh username@raspberrypi.local

Enter your credential, once in,  

    cd ~/klipper 
    make menuconfig

Config should look like this, F446 for newer model, F429 for older model.  
<img width="600" alt="octopus_f446_klipper_menuconfig" src="https://github.com/user-attachments/assets/dbb4b4a6-08f6-4ea4-8f8a-18f928370ba7" />
<img width="600" alt="octopus_f429_klipper_menuconfig" src="https://github.com/user-attachments/assets/1da9a875-9263-4f6e-9b25-51d759406dcc" />


<pre>Q(Quit)
Y(Yes)</pre>
    make clean
    make

You should see something like this:  
<img width="600" alt="螢幕擷取畫面 2025-08-18 194845" src="https://github.com/user-attachments/assets/6be64fa1-10f0-4902-b0d1-bb85088cd959" />


### Enableing DFU mode
Disconnect all peripherals from your BTT Octopus, install jumper on J75(BOOT0) and J68(V_Bus_USBC).  
<img width="800" alt="478787745-544924b4-5281-45a1-aa7d-26993cfd7183" src="https://github.com/user-attachments/assets/612bd681-6a7e-439b-a4f7-d02999f63740" />


Connect the A to C cable from host to BTT Octopus, **Hit reset** on BTT Ocpotus which is next to Type-C connector, find the ID for your BTT Octopus by running this commend, look for "STMicroelectronics STM Device in DFU Mode"  

    lsusb
<img width="600" alt="image" src="https://github.com/user-attachments/assets/42805ae0-653d-4c84-a904-48a028467a36" />  

Image credit: [Klipper Installation *NO SD CARD* PART 2, (DFU MODE)](https://youtu.be/NkSNBOTtZ6g?si=veryoF8daVonJYZ4)  

Flash the firmware with this commend, replace the placeholder with your ID

    make flash FLASH_DEVICE=XXXX:XXXX

If succeed, you should see something like this, you can ignore that error 255 as long as you have "File downloaded successfully".  
<img width="600" alt="螢幕擷取畫面 2025-08-18 194926" src="https://github.com/user-attachments/assets/7ab94e2b-cc0d-4fd6-8e39-ff92be6de3b3" />

Unplug your BTT Octopus, remove jumper from J75(BOOT0), and reconnect it to your host, run this to find your klipper device ID, should look like this.

    ls /dev/serial/by-id/*
<img width="600" alt="image" src="https://github.com/user-attachments/assets/0a169810-3c59-4167-8bf3-bd38918f5b31" />

Copy that and paste it in your printer.cfg as so:

    [mcu]
    serial: /dev/serial/by-id/usb-Klipper_stm32f446xx_XXXXXXXXXXXXXXXXXXXXXXXX-if00

Find reference printer.cfg for BTT Octopus [here](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/blob/master/Octopus%20works%20on%20Voron%20v2.4/Firmware/Klipper/BTT_OctoPus_Voron2_Config.cfg).  
For future updating your firmware, visit [Firmware Updates](https://docs.vorondesign.com/build/software/octopus_klipper.html#firmware-updates), or do this:  

    cd ~/klipper 
    make menuconfig
<img width="600" alt="octopus_f446_klipper_menuconfig" src="https://github.com/user-attachments/assets/dbb4b4a6-08f6-4ea4-8f8a-18f928370ba7" />  

<pre>Q(Quit)
Y(Yes)</pre>
    make clean
    make
    sudo service klipper stop
    cd ~/klipper
    make flash FLASH_DEVICE=/dev/serial/by-id/usb-Klipper_stm32f446xx_XXXXXXXXXXXXXXXXXXXXXXXX-if00
    sudo service klipper start

Happy printing.





