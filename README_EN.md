This is an automatic translation, may be incorrect in some places. See sources and examples!

# Fft_c
Fourier transformation library on C (for ESP8266)

## compatibility
Compatible with all arduino platforms (used arduino functions).But thought for ESP8266

## Content
- [installation] (# Install)
- [initialization] (#init)
- [use] (#usage)
- [Example] (# Example)
- [versions] (#varsions)
- [bugs and feedback] (#fedback)

<a id="install"> </a>
## Installation
- The library can be found by the name ** fft_c ** and installed through the library manager in:
    - Arduino ide
    - Arduino ide v2
    - Platformio
- [download the library] (https://github.com/gyverlibs/fft_c/archive/refs/heads/main.zip) .Zip archive for manual installation:
    - unpack and put in * C: \ Program Files (X86) \ Arduino \ Libraries * (Windows X64)
    - unpack and put in * C: \ Program Files \ Arduino \ Libraries * (Windows X32)
    - unpack and put in *documents/arduino/libraries/ *
    - (Arduino id) Automatic installation from. Zip: * sketch/connect the library/add .Zip library ... * and specify downloaded archive
- Read more detailed instructions for installing libraries [here] (https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%BD%D0%BE%BE%BE%BED0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Update
- I recommend always updating the library: errors and bugs are corrected in the new versions, as well as optimization and new features are added
- through the IDE library manager: find the library how to install and click "update"
- Manually: ** remove the folder with the old version **, and then put a new one in its place.“Replacement” cannot be done: sometimes in new versions, files that remain when replacing are deleted and can lead to errors!


<a id="init"> </a>
## initialization
No

<a id="usage"> </a>
## Usage
`` `CPP
#define fft_size 64 // before connecting the library.Should be a multiple of the degree of 2
VOID FFT (int* aval, int* ftvl);
// aval - an array of analyzed data
// ftvl - an array of obtained values
`` `

<a id="EXAMPLE"> </a>
## Example
The rest of the examples look at ** Examples **!
`` `CPP
// Transformation of Fourier on C (for ECP8266)

#define fft_size 64 // 32/64/128/256/512
#include <fft_c.h>
#include <volanayzer.h>
VOLANALYZER LOW, HIGH;


int raw [fft_size];
int spctr [fft_size];

#define strip_pin d4 // Pin tape
#define max_leds 512 // Max.LEDs
#define strip_chip ws2812 // chip tapes
#define strip_color GRB // Procedure of flowers in the tape
#include <Fastled.h> // Tape
CRGB LEDS [8];

VOID setup () {
  Fastled.addleds <strip_chip, strip_pin, strip_color> (Leds, 8);
  Fastled.Setbrightness (100);
  Serial.Begin (9600);

  Low.Setdt (0);
  Low.Setperiod (0);
  Low.Setwindow (0);
  High.Setdt (0);
  High.Setperiod (0);
  High.Setwindow (0);

  Low.Setvolk (25);
  High.Setvolk (25);
  Low.Settrsh (50);
  High.Settrsh (50);
  Low.Setvolmax (150);
  High.Setvolmax (150);
  Low.Setvolmin (30);
  High.Setvolmin (30);
}

VOID loop () {
  for (int i = 0; i <fft_size; i ++) raw [i] = analogread (a0);

  FFT (RAW, Spectr);
  int low_RAW = 0;
  Int High_Raw = 0;
  for (int i = 0; i <fft_size / 2; i ++) {
    spectr [i] = (spectr [i] * (i + 2)) >> 1;
    if (i <2) low_RAW += spectr [i];
    Else High_Raw += Spectr [i];
  }

  L.Cranberries ow.tick (low_RAW);
  High.tick (High_Raw);

  Fastled.clear ();
  // Fill_SOLID (LEDS, LOW.GETVOL () * 8 /150, ChSV (0, 255, 255));// volume strip
  Fill_SOLID (LEDS, 8, ChSV (0, 255, High.getvol ()));
  Fill_SOLID (LEDS, 4, ChSV (150, 255, Low.getvol ()));
  Fastled.show ();
  Serial.print (low.getvol ());
  Serial.print ('');
  Serial.println (High.getvol ());

  /*
    for (int i = 0; i <fft_size / 2; i ++) {
    if (spectr [i] <10) spectr [i] = 0;
    Serial.print (int (spectr [i]));
    Serial.print ('');
    }
    Serial.println ();
  */
}

`` `

<a id="versions"> </a>
## versions
- V1.0

<a id="feedback"> </a>
## bugs and feedback
Create ** Issue ** when you find the bugs, and better immediately write to the mail [alex@alexgyver.ru] (mailto: alex@alexgyver.ru)
The library is open for refinement and your ** pull Request ** 'ow!


When reporting about bugs or incorrect work of the library, it is necessary to indicate:
- The version of the library
- What is MK used
- SDK version (for ESP)
- version of Arduino ide
- whether the built -in examples work correctly, in which the functions and designs are used, leading to a bug in your code
- what code has been loaded, what work was expected from it and how it works in reality
- Ideally, attach the minimum code in which the bug is observed.Not a canvas of a thousand lines, but a minimum code