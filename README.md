# How-to-flash-klipper-firmware-on-BigTreeTech-Octopus
Instruction for flashing klipper firmware onto BTT Octopus via DFU mode.

## Setup
### For this guide, you'll need the following: ###

 - BTT Octopus
 - BTT Octopus's manual.
 - High quality USB Type-A to Type-C cable.
 - 24V DC for BTT Octopus
 - Two 0.1" pitch jumper

## Instruction
### Reference:  
**For flashing firmware:**  
https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master/Octopus%20works%20on%20Voron%20v2.4/Firmware/Klipper  
**For other stuffs:**  
https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/tree/master

Open PuTTY, or Powershell after 2018/01/10, SSH into your klipper

<pre>cd ~/klipper 
make menuconfig</pre>

Config should look like this, F446 for newer model, F429 for older model.  
<img width="600" alt="octopus_f446_klipper_menuconfig" src="https://github.com/user-attachments/assets/dbb4b4a6-08f6-4ea4-8f8a-18f928370ba7" />
<img width="600" alt="octopus_f429_klipper_menuconfig" src="https://github.com/user-attachments/assets/1da9a875-9263-4f6e-9b25-51d759406dcc" />


<pre>Q(Quit)
Y(Yes)</pre>

<pre>make clean
make</pre>

### Enableing DFU mode
Disconnect all peripherals from your BTT Octopus.  
Locate J75(Boot0) jumper, and install a jumper on J75.  
Locate J68(V_Bus_USBC) jumper, and install a jumper on J68 as well.  
<img width="800" alt="octopus_pinout" src="https://github.com/user-attachments/assets/544924b4-5281-45a1-aa7d-26993cfd7183" />  




