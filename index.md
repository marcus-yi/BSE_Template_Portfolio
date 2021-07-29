# Infinity Mirror Clock
Using LED lights to imitate the hour, minute and second hand normally seen in an analogous clock.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Marcus | Valley Christian | Undecided | Senior

[![Headstone Image](https://cdn.discordapp.com/attachments/865684000450412547/870169339788152872/image0.jpg)](https://www.youtube.com/watch?v=9Bqgl29xB1Y)
  
# Final Milestone
My final milestone is building the physical framework of the clock and putting the LED lights on the clock.

[![Final Milestone](https://cdn.discordapp.com/attachments/865684000450412547/870171832098099211/image0.png)](https://www.youtube.com/watch?v=wAUErA6igG4&feature=youtu.be)

# Second Milestone
I connected my RTC chip and my LED strip together. Now it displays the real time on my LED lights through a pattern.

```python
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
  while (!Serial);
#endif

  if (! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    Serial.flush();
    abort();
  }

  if (rtc.lostPower()) {
    Serial.println("RTC lost power, let's set the time!");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }
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
Video=:
[![Second Milestone](https://cdn.discordapp.com/attachments/865684000450412547/866568199305953280/Screen_Shot_2021-07-18_at_11.31.44_PM.png)](https://www.youtube.com/watch?v=z_nu1Ccq-Gk&feature=youtu.be)

# First Milestone

My first milestone is to get my RTC chip to work. I connected the wires RTC chip to the arduino, which in turn is connected to my laptop where I add the following code:

```python
#include "RTClib.h"

RTC_DS3231 rtc;

void setup () {
  Serial.begin(57600);

#ifndef ESP8266
  while (!Serial);
#endif

  if (! rtc.begin()) {
    Serial.println("Couldn't find RTC");
    Serial.flush();
    abort();
  }

  if (rtc.lostPower()) {
    Serial.println("RTC lost power, let's set the time!");
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }
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
After I uploaded this to the RTC, it was able to display real time up to the second. Video:

[![First Milestone](https://cdn.discordapp.com/attachments/865684000450412547/865695721691545621/IMG_1634.JPG)](https://www.youtube.com/watch?v=B-uA_yVBnaU)
