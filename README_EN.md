This is an automatic translation, may be incorrect in some places. See sources and examples!

# FFT_C
Fourier transform library in C (for esp8266)

### Compatibility
Compatible with all Arduino platforms (using Arduino functions). But I thought for esp8266

## Content
- [Install](#install)
- [Initialization](#init)
- [Usage](#usage)
- [Example](#example)
- [Versions](#versions)
- [Bugs and feedback](#feedback)

<a id="install"></a>
## Installation
- The library can be found by the name **FFT_C** and installed through the library manager in:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Download library](https://github.com/GyverLibs/FFT_C/archive/refs/heads/main.zip) .zip archive for manual installation:
    - Unzip and put in *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Unzip and put in *C:\Program Files\Arduino\libraries* (Windows x32)
    - Unpack and put in *Documents/Arduino/libraries/*
    - (Arduino IDE) automatic installation from .zip: *Sketch/Include library/Add .ZIP library…* and specify the downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE% D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Initialization
Not

<a id="usage"></a>
## Usage
```cpp
#define FFT_SIZE 64 // before including the library. Must be a multiple of a power of 2
void FFT(int* AVal, int* FTVl);
// AVal - array of analyzed data
// FTVl - array of received values
```

<a id="example"></a>
## Example
See **examples** for other examples!
```cpp
// Fourier transform in C (for esp8266)

#define FFT_SIZE 64 // 32/64/128/256/512
#include <FFT_C.h>
#include <VolAnalyzer.h>
VolAnalyzer low, high;


int raw[FFT_SIZE];
int spectr[FFT_SIZE];

#define STRIP_PIN D4 // strip pin
#define MAX_LEDS 512 // max. LEDs
#define STRIP_CHIP WS2812 // tape chip
#define STRIP_COLOR GRB // order of colors in the ribbon
#include <FastLED.h> // ribbon
CRGB leds[8];

void setup() {
  FastLED.addLeds<STRIP_CHIP, STRIP_PIN, STRIP_COLOR>(leds, 8);
  FastLED.setBrightness(100);
  Serial.begin(9600);

  low.setDt(0);
  low.setPeriod(0);
  low.setWindow(0);
  high.setDt(0);
  setPeriod(0);
  high.setWindow(0);

  low.setVolK(25);
  high.setVolK(25);
  low.setTrsh(50);
  high.setTrsh(50);
  low.setVolMax(150);
  high.setVolMax(150);
  low.setVolMin(30);
  high.setVolMin(30);
}

void loop() {
  for (int i = 0; i < FFT_SIZE; i++) raw[i] = analogRead(A0);

  FFT(raw, spectrum);
  int low_raw = 0;
  int high_raw = 0;
  for (int i = 0; i < FFT_SIZE / 2; i++) {
    spectr[i] = (spectr[i] * (i + 2)) >> 1;
    if (i < 2) low_raw += spectr[i];
    else high_raw += spectr[i];
  }

  low.tick(low_raw);
  high.tick(high_raw);

  FastLED.clear();
  //fill_solid(leds, low.getVol() * 8 / 150, CHSV(0, 255, 255)); // volume bar
  fill_solid(leds, 8, CHSV(0, 255, high.getVol()));
  fill_solid(leds, 4, CHSV(150, 255, low.getVol()));
  FastLED.show();
  Serial.print(low.getVol());
  Serial print(' ');
  Serial.println(high.getVol());

  /*
    for (int i = 0; i < FFT_SIZE / 2; i++) {
    if (spectr[i] < 10) spectr[i] = 0;
    Serial.print(int(spectr[i]));
    Serial print(' ');
    }
    Serial.println();
  */
}

```

<a id="versions"></a>
## Versions
- v1.0

<a id="feedback"></a>
## Bugs and feedback
When you find bugs, createCranberries are **Issue**, or better yet, write to [alex@alexgyver.ru](mailto:alex@alexgyver.ru)
The library is open for revision and your **Pull Request**'s!