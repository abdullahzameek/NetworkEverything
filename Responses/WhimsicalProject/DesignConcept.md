## Wifi-Controlled Jukebox (?)

So, recently I was playing around with my old Arduino Uno after I discovered a sound library for Processing called Minim. After consulting
a few online tutorials, I found a few to make LEDs blink in sync to a particular track being played.
My idea now, is to extend my previous project and make it in such a way it can be controlled from a webpage, and hopefully 
play more than one tune. Initially, I had to use StandardFirmata in conjunction with my Processing Sketch, but I hope that
I can find a workaround that doesn't involve the very un-friendly Firmata library programs.
For now, I have a concept in mind - one that might be a bit audacious at this stage, but I still want to give a shot and see how it 
goes.
For the time being, I'll upload a short video of what my wired prototype looks like.

Prototype - https://vimeo.com/255799592

## Attempt #1

So, for my first attempt at the project, I tried using Firmata with the Wi-fi functions to communicate with Processing. Unfortunately, it did not work and StandardFirmata no longer communicated with Processing. I then discovered a library called StandardFirmataWiFi, so I configured that library to work with my setup. However, once again, it failed to produce any results. 


At this point, I decided to drop the idea of using Firmata with Processing and I decided to use an MP3 Shield instead.

## Attempt #2 

For my second attempt, I tried to use the MP3 Shield with my MKR1000. At the time, I had no idea that the MP3 Shield requires an SPI pin to communicate with an Arduino, and I went on making the connections to the two via a breadboard. Unsurprisingly, it didnt work.

## Attempt #3

This time around, I decided to use an Arduino Uno, with a Wifi101 shield, and an MP3 Shield stacked on top of each other. The two shields  but they failed to work when I connected them altogether to the Arduino. Having spent several hours in frustration trying to figure out what the problem was, I approached Professor Aaron who then told me to test each component separately. Having done that, I came to realise that the shields worked when they were connected separately but not when they were together. After spending some time Googling the probable cause, I then found out that both the Wifi101 and MP3 shields communicate through the SPI pin, and the SPI pin can only communicate to one shield at a time. At this point of time, I had two options - one software-based and the other, hardware-based. 
1) I could start communication to the MP3 Shield, pause it midway, then communicate to the Wifi101 Shield, then disconnect from the Wifi101 shield, and resume communication to the MP3 Shield.
I considered this, but soon came to realise this to be impractical because the Wifi Shield needs to be communicated to constantly in order to establish and maintain a connection. Therefore, I abandoned this option and moved onto the hardware based solution which was Attempt #4

## Attempt #4

The hardware based solution is not the most elegant solution that exists, but it seemed reasonable in practice. If the Wifi Shield and MP3 Shield cannot work together, then the intuitive guess would be to connect them to their own Arduinos, and have the two Arduinos communicate with each other and act as the interface between to pass data to and from the shields. To facilitate this communication, I used Serial Communication i.e the RX/TX pins on the Arduinos. 
On one end, I had an MP3 Shield mounted on an Uno which was connected to a speaker, and on the other end, I had my MKR1000 which acted a server hosting the website to send input signals from. 
This is in essence, creating a network within a networked product, whereby everything is networked. Thus, in my project I had to literally "Network Everything".
However, it was still not over. I had to tackle multiple errors with regards to communicating between the two Arduinos.
i)Using Serial.begin() on the sketches on both sides didnt seem to work. After going through the documentation, I found out that Serial on a MKR1000 implies USB serial, while Serial1 implies serial communication via the RX/TX ports. Changing that fixed the issue.
ii)When a button was clicked, the client would send a value back to the server, for example, 'P' for play music, and then the MKR1000 would send the value to the other end. When it did the send the value however, it would send it 9600 times per second, and overwhelm the code on the other end. To circumvent this problem, I modified the code to only read a value only if it differed from the previous value read. This solved the problem.

At this point of time, the wireless MP3 Player is mostly functional and works fine under normal cases. I have to tackle a few edge and corner cases, but those are minor issues (volume not increasing, and delay in stopping a track).
The task was much more challenging than I had anticipated, but I'm glad that I went through it because a learnt so much about specific tecnical nitty-gritties which may come in use in the future.
