[![Foo](https://img.shields.io/badge/Version-1.0-brightgreen.svg?style=flat-square)](#versions)
[![Foo](https://img.shields.io/badge/Website-AlexGyver.ru-blue.svg?style=flat-square)](https://alexgyver.ru/)
[![Foo](https://img.shields.io/badge/%E2%82%BD$%E2%82%AC%20%D0%9D%D0%B0%20%D0%BF%D0%B8%D0%B2%D0%BE-%D1%81%20%D1%80%D1%8B%D0%B1%D0%BA%D0%BE%D0%B9-orange.svg?style=flat-square)](https://alexgyver.ru/support_alex/)

[![Foo](https://img.shields.io/badge/README-ENGLISH-brightgreen.svg?style=for-the-badge)](https://github-com.translate.goog/GyverLibs/FFT_C?_x_tr_sl=ru&_x_tr_tl=en)

# FFT_C
Библиотека преобразования Фурье на С (для esp8266)

### Совместимость
Совместима со всеми Arduino платформами (используются Arduino-функции). Но задумывалась для esp8266

## Содержание
- [Установка](#install)
- [Инициализация](#init)
- [Использование](#usage)
- [Пример](#example)
- [Версии](#versions)
- [Баги и обратная связь](#feedback)

<a id="install"></a>
## Установка
- Библиотеку можно найти по названию **FFT_C** и установить через менеджер библиотек в:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Скачать библиотеку](https://github.com/GyverLibs/FFT_C/archive/refs/heads/main.zip) .zip архивом для ручной установки:
    - Распаковать и положить в *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Распаковать и положить в *C:\Program Files\Arduino\libraries* (Windows x32)
    - Распаковать и положить в *Документы/Arduino/libraries/*
    - (Arduino IDE) автоматическая установка из .zip: *Скетч/Подключить библиотеку/Добавить .ZIP библиотеку…* и указать скачанный архив
- Читай более подробную инструкцию по установке библиотек [здесь](https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)

<a id="init"></a>
## Инициализация
Нет

<a id="usage"></a>
## Использование
```cpp
#define FFT_SIZE 64                 // перед подключением библиотеки. Должно быть кратно степени 2
void FFT(int* AVal, int* FTvl);
// AVal - массив анализируемых данных
// FTvl - массив полученных значений
```

<a id="example"></a>
## Пример
Остальные примеры смотри в **examples**!
```cpp
// преобразование Фурье на C (для есп8266)

#define FFT_SIZE 64  // 32/64/128/256/512
#include <FFT_C.h>
#include <VolAnalyzer.h>
VolAnalyzer low, high;


int raw[FFT_SIZE];
int spectr[FFT_SIZE];

#define STRIP_PIN D4        // пин ленты
#define MAX_LEDS 512        // макс. светодиодов
#define STRIP_CHIP WS2812   // чип ленты
#define STRIP_COLOR GRB     // порядок цветов в ленте
#include <FastLED.h>        // лента
CRGB leds[8];

void setup() {
  FastLED.addLeds<STRIP_CHIP, STRIP_PIN, STRIP_COLOR>(leds, 8);
  FastLED.setBrightness(100);
  Serial.begin(9600);

  low.setDt(0);
  low.setPeriod(0);
  low.setWindow(0);
  high.setDt(0);
  high.setPeriod(0);
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

  FFT(raw, spectr);
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
  //fill_solid(leds, low.getVol() * 8 / 150, CHSV(0, 255, 255));    // полоса громкости
  fill_solid(leds, 8, CHSV(0, 255, high.getVol()));
  fill_solid(leds, 4, CHSV(150, 255, low.getVol()));
  FastLED.show();
  Serial.print(low.getVol());
  Serial.print(' ');
  Serial.println(high.getVol());

  /*
    for (int i = 0; i < FFT_SIZE / 2; i++) {
    if (spectr[i] < 10) spectr[i] = 0;
    Serial.print(int(spectr[i]));
    Serial.print(' ');
    }
    Serial.println();
  */
}

```

<a id="versions"></a>
## Версии
- v1.0

<a id="feedback"></a>
## Баги и обратная связь
При нахождении багов создавайте **Issue**, а лучше сразу пишите на почту [alex@alexgyver.ru](mailto:alex@alexgyver.ru)  
Библиотека открыта для доработки и ваших **Pull Request**'ов!
