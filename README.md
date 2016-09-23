# homebridge-http

Supports https devices on the HomeBridge Platform and provides a readable callback for getting the "On" and brightness level characteristics to Homekit.

# About
This is a fork from  rudders/homebridge-http and is just really something i started playing with as i have been dabling with the arduino platform with relays and inputs to have the lights in my house available from my cell, as Homekit got released with the new IOS i made a few changes to my arduino code, and started using this plugin with my bridge. It has been great fun. Thanks to the original publisher for making it possible. I have a very limited understanding of the system and code, and am new to .js but have really leared alot from reading this and other plugins code.

# Installation

1. Install homebridge using: npm install -g homebridge
2. Install homebridge-http using: npm install -g homebridge-http
3. Update your configuration file. See sample-config.json in this repository for a sample. 

# Configuration

This module has recently been updated to support an additional method to read the power state of the device and the brightness level. Specify the `status_url` in your config.json that returns the status of the device as an integer (0 = off, 1 = on). 

Specify the `brightnesslvl_url` to return the current brightness level as an integer.

Switch Handling and brightness Handling support 3 methods namely: 

`yes`: 	(for polling on app load) is currently a cached hybrid it will look at the threshold in minutes and update the app accordingly
	this is to limit the time it takes for you to get multiple device, granted the first time you open the app after 5 minutes everything will request an update, after that it will serve the last result from disk.
	
`realtime`: will poll continusley this may cause some issues with this version, but we will look at resolving this if the requerement is 	there. it will currently go to error if there is a socket error, or if the connection times out for the status. This error will 		terminate the software.

`no`:	No polling, this is really the fasted way currently available. but has no status feedback.

`cached`: This may be implimented as the current yes depending on the comunity feedback, and the yes restored.


Configuration sample:

 ```
"accessories": [ 
	{
		"accessory": "Http",
		"name": "Alfresco Lamp",
		"switchHandling": "realtime",
		"http_method": "GET",
		"on_url":      "http://localhost/controller/1700/ON",
		"off_url":     "http://localhost/controller/1700/OFF",
		"status_url":  "http://localhost/status/100059",
		"service": "Light",
		"brightnessHandling": "yes",
		"brightness_url":     "http://localhost/controller/1707/%b",
		"brightnesslvl_url":  "http://localhost/status/100054",
		"sendimmediately": "",
		"username" : "",
		"password" : "",	
		
		"threshold" : 5
		
		
       } 
    ]
```

# http reply e.g.
What we expect for an HTTP Status request Here is how i render it

```
EthernetClient client = server.available();
client.println("HTTP/1.1 200 OK");
client.println("Content-Type: text/html");
client.println("Connnection: close");
client.println();
client.println(1)); // for on and 0 for off.
delay(1)
client.stop();
```          
   
Opening a web browser for the url should return the status, and just the status. [[ this may change if i read the reg ex fork]]
```   
0
```

It will not parse if there in no propper http 200 status, and it will return NaN if it is not 0 or 1 




#  Learn from reg ex fork.
#ToDo

Complete documentation and review a number of  forks

Write code nicely.
