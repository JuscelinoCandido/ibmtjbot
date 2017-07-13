# TJ-BOT for incident management

 TJBOT for incident management, is a project that showcase TJBOT's ability to monitor queues for incidents, it uses [Text-to-speech](https://www.ibm.com/watson/developercloud/text-to-speech.html), [Visual Recognition](https://www.ibm.com/watson/developercloud/visual-recognition.html) and [Twilio](https://console.bluemix.net/catalog/services/twilio/) APIs.
 When a new incident is received, TJBOT initiates the visual recognition API to identify if there is any operator on the floor. If a operator (face detection) is identifed, TJBOT will warm the operator by saying the ticket number, severity and client name. It also wave its arm and blink the led. 

 If TJBOT doesnt identify a person on the picture taken, it will initiate a cell phone call-out to the technical leader, using twilio APIs.

# TJBOT's Mood Marbles & weather report & twitter sentiment analysis

On this node-red flow, TJBOT also tracks the agile mood marbles from the team, tracks weather report and perform twitter sentiment analysis. The flow initiates when a movement is detected by the PIR sensor, which open the microphone to record speech. IBM watson speech-to-text API will transform the audio got from the microphone and look for specific keywords. 

If keyword sentiment is detected, the twitter sentiment analysis is executed by using twitter and [Tone-Analyser](https://www.ibm.com/watson/developercloud/tone-analyzer.html)

if keyword great, okay, angry is detected, a sum is done to the counter.

if keyword team is detected, TJBOT says how is the team's status.

if keyword weather is detected, TJBOT says the weather on the region defined on [Wether-Company](https://console.bluemix.net/docs/services/Weather/weather_overview.html#about_weather) API.

# IBM Workload Scheduler integration with TJBOT

IBM Workload Scheduler (IWS) ia a workload automation product, it automate the execution of multiple tasks running on a any number of servers on top of several different applications. Think as a powerful enterprise crontab companies use to control their automation.

One of IWS's functionalities is to orchestrate Internet Of Things devices running on [MQTT](https://start.wa.ibmserviceengage.com/ibm/TWSSandbox/wa/wa_new_info.jsp?dmy=no&video=QLGimYjpsg4&id=mqtt_info&cm_mc_uid=90793829266414999486338&cm_mc_sid_50200000=1499953211) protocol, IWS's has the capability to perform publish and subscribe. The main advantage is that I can control devices running MQTT protocol and integrate them to my batch flow (add complex dependencies, alerts, calendars).

Is this example, it was developed a integration between IWS & IBM IoT foundation, a event driven rule that monitors IBM Workload Scheduler environment looking for job abends was created, in case of any job abend detection a MQTT type job will be triggered. This job will post a message on IBM Bluemix IoT's foundation where TJBOT's is connected and listening for events. 

The message is dynamic, contain the abended job name and return code, uppon receival TJBOT will use Watson text-to-speech API to say the message and warn operations about the job abended. It is also integrated with Twilio to perform a call-out describing the event.

# Installation

This project was designed using node-red. In order to balance the capabilities (cloud vs local) most of it's processing and sensors were designed to be executed locally. 

There is multiple [instructables](https://www.instructables.com/member/TJBot/) guides to get you started with TJBOT, after you assemble your TJBOT and update node-red / npm you must install additional node-red modules, so the flow can run properly.


Node-red Module list:
 
rpi-ws281x-native

node-red-node-pi-neopixel (For led functions)

node-red-contrib-speakerpi (For speaker)

node-red-contrib-camerapi (For camera)

node-red-contrib-micropi (microfone capture)

node-red-node-twilio (Twilio node to perform call-out)

node-red-node-watson (Watson services APIs)

node-red-node-cf-cloudant (Cloudant database)

node-red-contrib-ibm-watson-iot (Watson iot service)

node-red-contrib-scx-ibmiotapp

[node-red-contrib-gpio](https://github.com/monteslu/node-red-contrib-gpio) (For Servo control)

[rasp-io](https://github.com/nebrius/raspi-io)

# PIR (Passive infrared) sensor install

This project contains a hardware modification on the original TJBOT design, a additional passive infrared (PIR) sensor was added on TJBOT's right eye, this is for the TJBOT's microphone to be opened only in case of detected motion. That avoid the mic to be opened at all times, which can considerable increase your IBM watson speech-to-text service quota.

![alt text](https://raw.github.ibm.com/jcandido/tjbot/master/raspberry-pi-3.png?token=AAGO0RVcZcWQVeUWSnfHgDHowqoRX81xks5ZYhbYwA%3D%3D)

