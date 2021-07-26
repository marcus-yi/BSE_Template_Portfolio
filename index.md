# Infinity Mirror Clock
This will serve as a brief description of your project. Limit this to three sentences because it can become overly long at that point. This copy should draw the user in and make she/him want to read more.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Marcus | Valley Christian | Undecided | Senior

![Headstone Image]()
  
# Final Milestone
My final milestone is building the physical framework of the clock and putting the LED lights on the clock.

[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612573869/video_to_markdown/images/youtube--F7M7imOVGug-c05b58ac6eb4c4700831b2b3070cd403.jpg )](https://www.youtube.com/watch?v=F7M7imOVGug&feature=emb_logo "Final Milestone"){:target="_blank" rel="noopener"}

# Second Milestone
I connected my RTC chip and my LED strip together. Now it displays the real time on my LED lights through a pattern.

```python
// Date and time functions using a DS3231 RTC connected via I2C and Wire lib
#include "RTClib.h"
#include <Adafruit_NeoPixel.h>

RTC_DS3231 rtc;
Adafruit_NeoPixel strip = Adafruit_NeoPixel (60, 6, NEO_GRB + NEO_KHZ800);

void setup () {
  Serial.begin(57600);
  strip.begin();
  strip.setBrightness(50);
  strip.show();

#ifndef ESP8266
  while (!Serial); // wait for serial port to connect. Needed for native USB
#endif

  if (! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    Serial.flush();
    abort();
  }

  if (rtc.lostPower()) {
    Serial.println("RTC lost power, let's set the time!");
    // When time needs to be set on a new device, or after a power loss, the
    // following line sets the RTC to the date & time this sketch was compiled
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
    // This line sets the RTC with an explicit date & time, for example to set
    // January 21, 2014 at 3am you would call:
    // rtc.adjust(DateTime(2014, 1, 21, 3, 0, 0));
  }

  // When time needs to be re-set on a previously configured device, the
  // following line sets the RTC to the date & time this sketch was compiled
  // rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  // This line sets the RTC with an explicit date & time, for example to set
  // January 21, 2014 at 3am you would call:
  // rtc.adjust(DateTime(2014, 1, 21, 3, 0, 0));
}

void loop () {
    DateTime now = rtc.now();

    int hour = now.hour();
    int min = now.minute();
    int sec = now.second();
    
    Serial.print(now.hour(), DEC);
    Serial.print(':');
    Serial.print(now.minute(), DEC);
    Serial.print(':');
    Serial.print(now.second(), DEC);
    Serial.println();

    Serial.println();
    delay(1000);

    for(int i = 0; i < 60; i++){
      strip.setPixelColor(i, strip.Color(255, 255, 255));
    }
    strip.show();

    strip.setPixelColor(sec, strip.Color(255, 0, 0));
    strip.setPixelColor(min, strip.Color(0, 255, 0));
    strip.setPixelColor((hour%12)*5, strip.Color(0, 0, 255));
    strip.show();
}
```
[![Second Milestone](https://cdn.discordapp.com/attachments/865684000450412547/866568199305953280/Screen_Shot_2021-07-18_at_11.31.44_PM.png)](https://www.youtube.com/watch?v=z_nu1Ccq-Gk&feature=youtu.be){:target="_blank" rel="noopener"}

# First Milestone

My first milestone is to get my RTC chip to work. I connected the wires RTC chip to the arduino, which in turn is connected to my laptop where I add the following code:

```python
// Date and time functions using a DS3231 RTC connected via I2C and Wire lib
#include "RTClib.h"

RTC_DS3231 rtc;

void setup () {
  Serial.begin(57600);

#ifndef ESP8266
  while (!Serial); // wait for serial port to connect. Needed for native USB
#endif

  if (! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    Serial.flush();
    abort();
  }

  if (rtc.lostPower()) {
    Serial.println("RTC lost power, let's set the time!");
    // When time needs to be set on a new device, or after a power loss, the
    // following line sets the RTC to the date & time this sketch was compiled
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
    // This line sets the RTC with an explicit date & time, for example to set
    // January 21, 2014 at 3am you would call:
    // rtc.adjust(DateTime(2014, 1, 21, 3, 0, 0));
  }

  // When time needs to be re-set on a previously configured device, the
  // following line sets the RTC to the date & time this sketch was compiled
  // rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  // This line sets the RTC with an explicit date & time, for example to set
  // January 21, 2014 at 3am you would call:
  // rtc.adjust(DateTime(2014, 1, 21, 3, 0, 0));
}

void loop () {
    DateTime now = rtc.now();

    Serial.print(now.hour(), DEC);
    Serial.print(':');
    Serial.print(now.minute(), DEC);
    Serial.print(':');
    Serial.print(now.second(), DEC);
    Serial.println();

    Serial.println();
    delay(1000);
}
```
After I uploaded this to the RTC, it was able to display real time up to the second.

[![First Milestone](https://cdn.discordapp.com/attachments/865684000450412547/865695721691545621/IMG_1634.JPG)](https://www.youtube.com/watch?v=B-uA_yVBnaU){:target="_blank" rel="noopener"}
