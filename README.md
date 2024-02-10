# Ethernet Teleinfo Shield for Olimex ESP32 POE

<img src="https://github.com/hallard/EthTinfo/blob/main/pictures/EthTinfo-assembled.jpg" alt="Ethernet Teleinfo on Olimex ESP32 POE">

This shield is to be combined with [Olimex ESP32 POE board](https://www.olimex.com/Products/IoT/ESP32/ESP32-POE/open-source-hardware) to get French energy meter called Teleinfo (AKA TIC) 


It is mainly needed when your smartmeter his far from house and WiFi signal too weak and/or if you don't have power near your smartmeter to power Denky, WeMos Teleinfo or any other type of board. Having Ethernet near smart mater and you done with POE, Olimex board and this Shield.


It has the following features:

- Teleinfo Reader interface
- Blue LED to see if Teleinfo signal is received
- Trimmer to allow TIC sensitivity change without soldering resistors
- RGB User Led

It is a plug and play board, nothing to solder or assemble.

# History

<!-- V1.1
**v1.1**
- Route RGB Led to GPIO5 
-->

**v1.0**

- First fully working prototype

# Detailed Description

## Connectivity

The shield is connected thru Olimex UExt connector

<img src="https://github.com/hallard/EthTinfo/blob/main/pictures/olimex-uext.jpg" alt="Olimex UEXT conenctor">

Look at the schematics for more informations, easy to understand. Wiring is as follow:

| Pin Function | ESP32 | 
|  :---        | :---: | 
| Téléinfo Rx  | GPIO36 | 
| RGB Led      | GPIO2 | 

<!-- V1.1
| RGB Led      | GPIO5 | 
-->

Unfortunately RGB led share the same pin than SD Card GPIO2 so if you want to use RGB LED you need to disable SD Card GPIO mapping and vice versa

# Schematics

<img src="https://github.com/hallard/EthTinfo/blob/main/pictures/EthTinfo-sch.png" alt="Ethernet Teleinfo Schematics">

# Boards

<img src="https://github.com/hallard/EthTinfo/blob/main/pictures/EthTinfo-top.jpg" alt="Ethernet Teleinfo Top">

<img src="https://github.com/hallard/EthTinfo/blob/main/pictures/EthTinfo-bot.jpg" alt="Ethernet Teleinfo Bottom">


# Firmware 

It's compatible with Arduino IDE, Visual Studio Code and also Micro Python.

You can write your own and use with [LibTeleinfo](https://github.com/hallard/LibTeleinfo) library I wrote if you want to get rid of driving teleinfo stuff (chekout some [examples](ttps://github.com/hallard/LibTeleinfo/tree/master/examples)).

## Tasmota

But I strongly suggest using amazing [Tasmota](https://tasmota.github.io/docs/) firmware, all is already done and well done.

Please check Teleinfo official tasmota [documentation](https://tasmota.github.io/docs/Teleinfo/) so see how to configure your device depending on smartmeter type and what options you need.

### Nicolas's builds

Nicolas @NicolasBernaerts has made awesome changes on Tasmota to be able to show graph of consumption, history and other great features such has:

- Ecowatt server to publish RTE Ecowatt signals
- TCP server to live stream teleinfo data
- FTP server to easily retrieve graph data

You can find more information on his dedicated [repo](https://github.com/NicolasBernaerts/tasmota/tree/master/teleinfo) and also ready made build.

### Tasmota Unofficial builds

Teleinfo is not member of official Tasmota builds. So you need to build your own with `USE_TELEINFO` define. 
But Tasmota team agreed it's not simple to build for end users, and they now provide unofficial build that follow developement branch, this mean you always have an up to date build with latest code available. Of course we added Teleinfo (ESP8266 and ESP32) builds in this unofficial build process so you have nothing to install and compile/link (building). You can't go easier.

And as a cherry on the cake, easy flasher tools (web version and executable one) will present Teleinfo firmware so you are able to flash teleinfo firmware in less than 1 minute. You can check detail [here](https://github.com/Jason2866/Tasmota-specials) but here how to do that.

- Launch [Web Flasher here](https://jason2866.github.io/Tasmota-specials/) 
- Select Teleinfo 
- Select Serial port, and click `install`

<img src="https://github.com/hallard/WeMos-TIC/raw/master/pictures/WeMos-TIC-web_flasher.png">

Once done something like that

<img src="https://github.com/hallard/WeMos-TIC/raw/master/pictures/WeMos-TIC-web_flasher_ok.png">

After flashed, you should now see a new access point named `tasmota_aabbcc_xxxx` where you can connect to configure your WiFi for the device to connect on.

Alternatively, if you connect serial console and reset the device you should see Serial logs like that
```
[12:38:28]00:00:00.001 HDW: ESP32-D0WD-V3 v3.0 
[12:38:28]00:00:00.079 UFS: FlashFS mounted with 312 kB free
[12:38:28]00:00:00.080 TFS: File '.settings' not found
[12:38:28]00:00:00.082 TFS: File '.settings.lkg' not found
[12:38:28]00:00:00.082 CFG: Use defaults
[12:38:28]00:00:00.193 QPC: Reset
[12:38:29]00:00:00.415 BRY: Berry initialized, RAM used 4540 bytes
[12:38:29]00:00:00.427 Project tasmota - Tasmota Version 13.2.0.3(teleinfo)-2_0_14(2023-12-05T15:04:33)
[12:38:29]00:00:00.532 WIF: WifiManager active for 3 minutes
[12:38:29]00:00:01.064 HTP: Web server active on tasmota-88D0C4-4292 with IP address 192.168.4.1
```

If you want to deep into this process or just curious, you can check out it's [here](https://github.com/Jason2866/Tasmota-specials)

:memo: If you have some issues flashing with Web Flasher, do not hesitate to use another awesome tool [ESP Flasher](https://github.com/Jason2866/ESP_Flasher), with this one you can see exactly what's going on in case of issue because it has built in console. Usefull also after reboot of the device because console still active. This is the one I'm using day by day. In this case you need to download firmware to flash first [here](https://github.com/Jason2866/Tasmota-specials/tree/firmware/firmware/tasmota/other). Download firmware `tasmota-teleinfo`

You can take a look on `autoconf` [folder](https://github.com/tasmota/autoconf/tree/main/raw/esp32/EthTinfo_V1.0) to see some of init commands used on EthTinfo.

:memo: Don't forget to reset Energy counters on first boot with console command `EnergyTotal 0` if you have erratic values. 

### Autoconf 

Another awesome feature of Tasmota is the ability to download configuration profile, and guess what, we done it for EthTinfo, just go to configuration option, select Autoconfig and then choose in the list `EthTinfo` and here you are, ne need to copy/paste template, it's done by autoconfig.
If you want to deep into this process or just curious, you can check out it's [here](https://github.com/tasmota/autoconf)


### Tasmota template

I strongly suggest using awesome Tamsota autoconf feature but if you need Tasmota template for EThTinfo, here it it

<!--
#### V1.1 

With SD Card and RGB Led
```json
{"NAME":"EthTinfo (Olimex ESP32)","GPIO":[1,1,8864,1,0,1376,0,0,5536,1,8832,8800,0,0,5600,1,1,1,1,5568,1,1,1,1,0,0,0,0,1,1,32,1,5632,1,1,1],"FLAG":0,"BASE":1}
```
-->

#### V1.0 With SD Card (No RGB Led)

```json
{"NAME":"EthTinfo (Olimex ESP32)","GPIO":[1,1,8864,1,0,1,0,0,5536,1,8832,8800,0,0,5600,1,1,1,1,5568,1,1,1,1,0,0,0,0,1,1,32,1,5632,1,1,1],"FLAG":0,"BASE":1}
```

#### V1.0 With RGB Led (No SD Card)

```json
{"NAME":"EthTinfo (Olimex ESP32)","GPIO":[1,1,1376,1,0,1,0,0,5536,1,1,1,0,0,5600,1,1,1,1,5568,1,1,1,1,0,0,0,0,1,1,32,1,5632,1,1,1],"FLAG":0,"BASE":1}
```


## Berry Scripting 

Now you can personalize code with [Berry language](https://tasmota.github.io/docs/Berry/). Check out some Berry samples [here](https://github.com/arendst/Tasmota/blob/development/tasmota/berry/examples/)

You can do that by going to Berry console from Tasmota WEB user interface.

### Drive RGB LED depending on actual power

Here is a Berry example, goal is to follow real time consumption driving on board RGB Led depending on current Power consumption (low green then going to red when reaching maximum current of your contract)

```python
#-
# example of using Berry script to change the led color
# accordingly to power consumption
# using Denky, WeMos Teleifno or EthTinfo (French Teleinfo reader)
-#

#- define the global symbol for reference -#
runcolor = nil

def runcolor()
  var max_contrat = 30 # contrat 30A
  var i = energy.current
  #print(i)
  var red = tasmota.scale_uint(int(i), 0, max_contrat, 0, 255)
  var green = 255 - red
  var channels = [red, green, 0]
  light.set({"channels":channels, "bri":64, "power":true})
  tasmota.set_timer(2000, runcolor)
end

#- run animation -#
runcolor()
```

### Send data to Emoncms with Berry 

What's magic with Berry is the ability to do basic stuff with data, in this example we will intercept MQTT send message by Energy driver, do some calc and send data to Emoncms every 15 seconds and also to drive RGB Led from Green (low load) to Red (approach max subscription)

Modifiy API key with your, and copy paste the following code into Berry Console. Tst and validate if all is okay for you.

Once all is fine, you paste the code into a file named `autoexec.be` on the Tasmota Filesystem so it will be executed each time Tasmota device starting.

#### Mode Historique (contrat heures creuses)

```python
import json

var api_url = "https://emoncms.org/input/post"
var api_key = "YOUR_EMON_API_WRITE_KEY"
var node_name = "NODE_NAME"
var post_every = 15000 # post evert 15 seconds
var payload = {}

def send_emoncms()
  # Convert JSON object to string 
  var obj_json = json.dump(payload)

  # Create URL to call
  var param="?fulljson="+obj_json + "&node="+node_name + "&apikey="+api_key 
  # Post Data to EMONCMS
  var cl = webclient()
  cl.begin( api_url + param)
  var r =  cl.GET()
  tasmota.set_timer(post_every, send_emoncms)
end

def setcolor(iinst, isousc)
  var red = tasmota.scale_uint(iinst, 0, isousc, 0, 255)
  var green = 255 - red
  var channels = [red, green, 0]
  light.set({"channels":channels, "bri":64, "power":true})
end

# set global payload the field we need
def rule_tic(value, trigger)
  # Got Heures Creuses contract so I will calculate total consumption
  # adding Heures Creuses (HCHC) + Heures Pleines (HCHP) and create new value for emoncms 
  # Change label depending on name for your contract type
  var htot = value['HCHP'] + value['HCHC'] # mode historique
  # Create new value HTOT converted to kWH
  payload['HTOT'] = htot / 1000.0
  # Calculate current percent Load 
  var iinst = value['IINST']  # mode historique
  var isousc= value['ISOUSC'] # mode historique

  if iinst != nil && isousc != nil 
    # Drive RGB LED
    setcolor(iinst, isousc)
    if isousc > 0
      load = 100 * iinst / isousc
      payload['LOAD'] = load
    end
  end

  # Set values we need to send to emoncms
  payload['ADCO']  = value['ADCO']
  payload['HCHP']  = value['HCHP']
  payload['HCHC']  = value['HCHC']
  payload['ISOUSC']= isousc
  payload['PAPP']  = value['PAPP']
  payload['IINST'] = iinst

end

def start()
  # Callback on each frame interception
  tasmota.add_rule("TIC",rule_tic)
  # fire 1st post in 5s 
  tasmota.set_timer(5000, send_emoncms)
end

# delay start to have time to get full frame
tasmota.set_timer(10000, start)

```


#### Mode Standard (contrat heures creuses)

```python
import json

var api_url = "https://emoncms.org/input/post"
var api_key = "YOUR_EMON_API_WRITE_KEY"
var node_name = "NODE_NAME"
var post_every = 15000 # post evert 15 seconds
var payload = {}

def send_emoncms()
  # Convert JSON object to string 
  var obj_json = json.dump(payload)

  # Create URL to call
  var param="?fulljson="+obj_json + "&node="+node_name + "&apikey="+api_key 
  # Post Data to EMONCMS
  var cl = webclient()
  cl.begin( api_url + param)
  var r =  cl.GET()
  tasmota.set_timer(post_every, send_emoncms)
end

def setcolor(iinst, isousc)
  var red = tasmota.scale_uint(iinst, 0, isousc, 0, 255)
  var green = 255 - red
  var channels = [red, green, 0]
  light.set({"channels":channels, "bri":64, "power":true})
end

# set global payload the field we need
def rule_tic(value, trigger)
  # Got Heures Creuses contract so I will calculate total consumption
  # Calculate current percent Load 
  var iinst = value['IRMS1']  
  var isousc= value['PREF']*5 

  if iinst != nil && isousc != nil 
    # Drive RGB LED
    setcolor(iinst, isousc)
    if isousc > 0
      load = 100 * iinst / isousc
      payload['LOAD'] = load
    end
  end

  # Here I keep name of historique mode
  payload['ADCO']  = value['ADSC']
  payload['HTOT']  = value['EAST'] / 1000 # I need kWh
  payload['HCHP']  = value['EASF01']
  payload['HCHC']  = value['EASF02']
  payload['ISOUSC']= isousc
  payload['PAPP']  = value['SINSTS']
  payload['IINST'] = iinst

end

def start()
  # Callback on each frame interception
  tasmota.add_rule("TIC",rule_tic)
  # fire 1st post in 5s 
  tasmota.set_timer(5000, send_emoncms)
end

# delay start to have time to get full frame
tasmota.set_timer(10000, start)

```


### Custom WEB Interface

If you are not happy with values on the WEB interface you can totaly change them to fit your needs.

You can add new values to display or disable all values displayed and use you own or even some calculated ones.

To disable all current values displayed please use the following command in console (not the berry one)

```
backlog0 WebSensor0 0 ; WebSensor1 0 ; WebSensor2 0 ; WebSensor3 0
```

Then you can add Teleinfo driver (this one as example) into a file named `teleinfo.be`

This is just an example please fill it to your needs. This one display Total `EAST` index, `URMS1` and `IRMS1` on web interface


```python
# Teleinfo driver written in Berry
# Support for Teleinfo custom values from choosen fields returned by smart meter
 
class TELEINFO: Driver

  # Global var Needed
  var east
  var irms
  var urms

  def init()
    # initialize globals

    # Don't display original sensors on WebUI, we want just our ones
    tasmota.cmd("backlog0 WebSensor0 0 ; WebSensor1 0 ; WebSensor2 0 ; WebSensor3 0")

    # create rules to trigger when Teleinfo values updates 
    # Use Teleinfo Etiquette Names to get whatever value you need
    tasmota.add_rule("TIC#EAST", /value -> self.trigger_east(value))
    tasmota.add_rule("TIC#IRMS1", /value -> self.trigger_irms(value))
    tasmota.add_rule("TIC#URMS1", /value -> self.trigger_urms(value))
  end

  def trigger_east(index)
    self.east = index
  end

  def trigger_irms(i)
    self.irms = i
    # DEBUG print("irms:", i)
  end

  def trigger_urms(u)
    self.urms = u
    # DEBUG print("urms:", u)
  end

  # trigger a read every second 
  def every_second()
    # DEBUG print("sensors:",tasmota.read_sensors())
  end

  # display sensor value in the web UI
  def web_sensor()
    import string
    var msg = string.format(
            "{s}Consommation{m}%d Wh{e}"
            "{s}Tension RMS{m}%d V{e}"
            "{s}Intensité RMS{m}%d A{e}", 
             self.east, self.urms, self.irms )

    tasmota.web_send_decimal(msg)
  end

end

# Instantiate the driver
teleinfo = TELEINFO()
# add it to Tasmota
tasmota.add_driver(teleinfo)

```

Then you need to load and start it into your `autoexec.be` file. just add followin at the end of the file

```python
# Auto start teleinfo driver
load("teleinfo.be")
```

#### Here how it looks like

<img src="https://github.com/hallard/Denky-D4/blob/main/pictures/Denky-D4-tasmota-custom-ui.png" alt="Ethernet Teleinfo Custom User Interface">


## Working board 

<img src="https://github.com/hallard/EthTinfo/blob/main/pictures/EthTinfo-working.jpg" alt="Ethernet Teleinfo Working on Olimex ESP32 POE">

# Support and discussion

If you have any issue or just want to discuss on this project, please use community [forum](https://community.ch2i.eu/category/20)

# License

<img alt="Creative Commons Attribution-NonCommercial 4.0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png">   

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/)    
If you want to do commercial stuff with this project, please contact [CH2i company](https://ch2i.eu/en#support) so we can organize an simple agreement.


# Lazy building your own? 

You can order this module fully assembled with some extra on [tindie][1]
<a href="https://www.tindie.com/products/33668/"><img src="https://d2ss6ovg47m0r5.cloudfront.net/badges/tindie-mediums.png" alt="I sell on Tindie" width="150" height="78"></a>

# Misc

See news and other projects on my [blog][2] 
 
[1]: https://www.tindie.com/products/33668/
[2]: https://hallard.me










