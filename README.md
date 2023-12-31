# SenseCAP-M1-Helium-Gateway-to-TTN
Contains configurations file and my learnings for Australians (AS923 only at this stage) performing the Seeed SenseCAP M1 Helium gateway (WM1302 SPI LoRaWAN Module) conversion to The Things Network TTN basic station.

Follow the guide here: https://github.com/seeed-lora/WM1302-doc for conerting your SenseCAP M1 Helium gateway to a TTN gateway.

For what it's worth:
* I used a new microSD card in my SenseCAP M1 gateway and used the standard Raspberry Pi Imager from here: https://www.raspberrypi.com/software/ and did a "Raspberry Pi OS (other)" -> "Raspberry Pi OS Lite (64-bit)" image from that app's menu. 
* I set my WiFi and enabled SSH using this tool also.

If you're an Australian user, here's what you need to do:
1. When performing the **nano tools/reset_lgw.sh** edits at Step 4 (https://github.com/seeed-lora/WM1302-doc#step4-run-semtech-sx1302-packet-forwarder), my GPIO changes looked like this to work:

```
SX1302_RESET_PIN=17     # SX1302 reset
SX1302_POWER_EN_PIN=18  # SX1302 power enable
SX1261_RESET_PIN=5     # SX1261 reset (LBT / Spectral Scan)
AD5338R_RESET_PIN=13    # AD5338R reset (full-duplex CN490 reference design)
```

Further down in Step 4 of that guide, when it comes to editing the global_conf.json.sx1250.xxxx files, do the following instead:

2. In the **~/sx1302_hal/packet_forwarder $**  directory, do a **cp global_conf.json.sx1250.AS923.USB global_conf.json.sx1250.AS923** command. This will copy/create a new file that we can use to copy the contents of my file in this repo into.
3. Open my **global_conf.json.sx1250.AS923** file in Notepad++, edit the "gateway_ID": "**YOURGATEWAYEUIHERE**" to be the new EUI created with https://descartes.co.uk/CreateEUIKey.html (same EUI you will be manually setting in the TTN console). You don't have to change the default server address/ports etc. as I have that already set in my conf file for the Australian TTN broker.
4. Copy the whole contents of this from Notepad++ into the **global_conf.json.sx1250.AS923** in your SenseCAP M1 gateway. Write it out to save it.
5. When it comes to starting the lora packet forwarder, your command will be **./lora_pkt_fwd -c global_conf.json.sx1250.AS923**
6. You may also need to do a **./reset_lgw.sh** start if things don't work as expected at this stage.


Other:
* I have seen comments in the thethingsnetwork.org forums indicating that you can download the global.conf and I guess "use that". The problem is that this file from TTN console doesn't contain what's needed for the _lora_pkt_fwd_ service to initialise the SPI interface. Maybe I'm missing something - who knows..
* One day I might do the AU915 version of this. I'm happy for someone else to and I can test :) 
