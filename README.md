# gethue
A simple API to control Philips Hue lamps with http GET requests

# Background
The existing Philips Hue REST API required a PUT request to control the lights. 
I needed GET, so I made a simple API to translate from GET to PUT.

# Getting your Philips Hue API Key
If you have [Homebridge](https://homebridge.io/), and the [homebridge-hue](https://github.com/ebaauw/homebridge-hue) plugin, look at the **users** section of the hue config. You will see the Hue bridge MAC address folowed by the Hue bridge API key
```
"users": {
  "ECB5FAFFFEFFFFFF": "yourPhilipsHueApiKey"
 },
```
The API key will look something like this:
```
UBxWZChHseyjeFwAkwgbdQ08x9XASWpanZZVg-mj
```

            
# Installing gethue
To see the help file, start gethue without any arguments as follows:
```
node gethue
```


# Using gethue
Enter a URL (in the format shown below) into your browser and press Enter. Some examples:

* Turn light 31 on: http://192.168.x.x:3000/api/yourPhilipsHueApiKey/lights/31/state?on=true
* Turn light 31 off: http://192.168.x.x:3000/api/yourPhilipsHueApiKey/lights/31/state?on=false

* Turn light 31 on at 50% brightness: http://192.168.x.x:3000/api/yourPhilipshueApiKey/lights/31/state?on=true&bri=50
* Turn light 31 on at 100% brightness: http://192.168.x.x:3000/api/yourPhilipshueApiKey/lights/31/state?on=true&bri=100
* Identify light 31 with a single flash: http://192.168.x.x:3000/api/yourPhilipshueApiKey/lights/31/state?alert=select
* Identify light 31 with a long number of flashes (about 15): http://192.168.x.x:3000/api/yourPhilipshueApiKey/lights/31/state?alert=lselect

## Supported keywords
The API is transparent to all keywords, but it is a simple API. It does not do any nesting of JSON syntax, thus please expect only the simple light controls for the light **state** to work.

The full JSON response for a light looks like this:
```
{"1":{"state":{"on":false,"bri":198,"hue":5360,"sat":192,"effect":"none","xy":[0.5330,0.3870],"ct":500,"alert":"select","colormode":"xy","mode":"homeautomation","reachable":true},"swupdate":{"state":"noupdates","lastinstall":"2021-08-21T01:50:00"},"type":"Extended color light","name":"Standard Lamp","modelid":"LCA001","manufacturername":"Signify Netherlands B.V.","productname":"Hue color lamp","capabilities":{"certified":true,"control":{"mindimlevel":200,"maxlumen":800,"colorgamuttype":"C","colorgamut":[[0.6915,0.3083],[0.1700,0.7000],[0.1532,0.0475]],"ct":{"min":153,"max":500}},"streaming":{"renderer":true,"proxy":true}},"config":{"archetype":"floorshade","function":"mixed","direction":"omnidirectional","startup":{"mode":"safety","configured":true}},"uniqueid":"00:17:88:01:08:ff:ff:ff-0b","swversion":"1.90.1","swconfigid":"35F80D40","productid":"Philips-LCA001-4-A19ECLv6"}
```
As you can see, the available simple keywords for state are:
on,bri,hue,sat,effect,ct,alert,colormode,mode

For full details of the control capabilities, please see the [official Philips Hue API reference](https://developers.meethue.com/develop/hue-api/).
An [alternative reference](http://www.burgestrand.se/hue-api/) also exists.

# Finding your light ids
You need to know the light id of the light you wish to control.
Go to http://192.168.x.x/api/yourPhilipsHueApiKey/lights. You will see a responce that looks like this (truncated here for brevity):
```
{"1":{"state":{"on":false,"bri":198,"hue":5360,"sat":192,"effect":"none","xy":[0.5330,0.3870],"ct":500," ...
```
Copy and paste the response into a text editor and format as JSON (or use an online JSON display tool). Then search for the name of the light (as shown in the Home app) in the text response, here I searched for "Desk lamp":
```
... ,"type":"Extended color light","name":"Desk lamp","modelid":"LCT012", ...
```
Go backwards in the text until you find the keyword **state**, this is at the start of the JSON text for the light. The light id is the number immediately before state. In this case, my light id is 31:
```
... ,"31":{"state":{"on":true,"bri":100,"hue":65396 ...
```
