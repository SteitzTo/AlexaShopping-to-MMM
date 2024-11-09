# AlexaShopping-to-MMM

After I had installed a MagicMirror into my kitchen cabinet as an infotainment display about 4 years ago, the desire to present my Alexa shopping list there grew.

There was no corresponding module for MagicMirror, it remained with this request.

Since I have been working a lot lately with homeautomation/NodeRed/MQTT, and have integrated various things at home, I decided to do this entirely via NodeRed/MQTT and the MMM-MQTT module:

# Dependencies / assumption:

![Node Red](https://img.shields.io/badge/Node--Red-8F0000?style=for-the-badge&logo=nodered&logoColor=white)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NodeRed<br>
![Mosquitto](https://img.shields.io/badge/mosquitto-%233C5280.svg?style=for-the-badge&logo=eclipsemosquitto&logoColor=white)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Working Mosquitto as a MQTT-Broker<br>
![Amazon](https://a11ybadges.com/badge?logo=amazon) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Amazon account<br>
![Amazon Alexa](https://img.shields.io/badge/amazon%20alexa-52b5f7?style=for-the-badge&logo=amazon%20alexa&logoColor=white) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alexa<br>

# The following nodes in NodeRed:

[node-red-contrib-alexa-remote2-applestrudel](https://flows.nodered.org/node/node-red-contrib-alexa-remote2-applestrudel)<br>

[MQTT Node](https://cookbook.nodered.org/mqtt/connect-to-broker)

# For the MagicMirror:

[MMM-MQTT Module](https://github.com/ottopaulsen/MMM-MQTT)<br>


# For Node-Red 

Download the AlexShopping_flow.json from above

![Alexa_shoppinglist](https://github.com/user-attachments/assets/f957acfa-3ea5-4234-abe9-861f677411df)



# 1. Install the Node-Red Flow


Instructions for node-red-contrib-alexa-remote2-applestrudel:

The establishment of the Amazon account is a bit tricky, so here a small instructions as I have successfully made:

Install the node-red-contrib-alexa-remote2-applestrudel module from the Repository in Node-Red

Select the node Alexa List:

Create a new Alexa connection:

At the back of the plus symbol:

Name: e.g. Alexa1

Auth method: proxy

This IP: Address of the machine on the Node-Red runs e.g. localhost or 192.168.x.x

Port: 3456
If you want to integrate several Alexa instances, please different ports per instance, 
since all instances have to be authenticated independently and have to run on different ports.

Filepath: Path and file name of the cookies:

e.g. /home/pi/alexa_cookie1.txt

With several instances, a different cookie path must be created
e.g. /home/pi/alexa_cookie2.txt

The file must be specified, it is created automatically and filled with content.

Set the settings for Amazon as from the list [here](https://flows.nodered.org/node/node-red-contrib-alexa-remote2-applestrudel). I took over the rest of the settings.

Then add on the top and click on Deploy in Node-Red.

Now open the browser on the machine where Node-Red is running and enter the browser line:

http://localhost:3456

Important: The call must take place on the machine from Node-Red and not via the network on another computer, otherwise the Auth Cookie cannot be written.

With several Alexa instances, it should be done exactly like this, since each instance requires its own auth cookie. Think of the changed port numbers at several authorities.

Now you should get to an Amazon side to authenticate yourself.
I did this with my cell phone number, you get an SMS with a security code that you enter.

Now the message should appear in the browser window:

"Amazon Alexa Cookie Successfully Retrieved. You can close the browser. "

If a file with the name was created under the path for the cookie, it should have worked.

In the Node Alexa List you should now select your Alexa account and under Select: “Get-List-Items” and then select the shopping list underneath.

# 2. Install the MMM-MQTT Module in your Magic Mirror

Magic Mirror Config for MMM-MQTT:

clone Git repository in the module directory and install

git clone https://github.com/ottopaulsen/MMM-MQTT

Config.js:
```
config: {
            logging: false,
            useWildcards: true,
            bigMode: false, // Set to true to display big numbers with label above
            mqttServers: [
                           {
                              address: '192.168.2.2',  // Server address or IP address of the MQTT Broker
                              port: '1883',          // Port number if other than default
                              user: '*******',          // Leave out for no user
                              password: '******',      // Leave out for no password
                              subscriptions: [											
													{
                                                    topic: 'Alexa/Torsten/Einkaufsliste', // Topic to look for											
                                                    label: 'Einkaufsliste', // Displayed in front of value                                                   
                                                    sortOrder: 10,        // Can be used to sort entries in the same table                                                   
                                                    },
													
													
                                              ],
                            },
                        ],
           },
},

```

It takes a while for the entries to appear in MagicMirror, depending on the interval as it is set in Node-Red. For testing I set 5sek, 1min in normal operation.

The first 10 entries of the shopping list, which are not yet ticked off, are shown.

![1729159299953-20241017_115553](https://github.com/user-attachments/assets/f14c1884-00df-41f8-ad0c-693a52b5ae35)


Hope i can help anybody who have the same wish…

Greetings Torsten

