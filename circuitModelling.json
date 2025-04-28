#include <Wire.h>
#include "Keypad.h"
#include "SPI.h"
#include "Adafruit_GFX.h"
#include "Adafruit_ILI9341.h"
#include <LiquidCrystal_I2C.h>

#define TFT_DC D15
#define TFT_CS D10
Adafruit_ILI9341 tft = Adafruit_ILI9341(TFT_CS, TFT_DC);

#define MAXBUFFERSIZE 20
#define BUTTON_PIN D14
#define DEBOUNCE_DELAY 500UL  


const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  { '1','2','3','A' },
  { '4','5','6','B' },
  { '7','8','9','C' },
  { '*','0','#','D' }
};

byte rowPins[ROWS] = { D9, D8, D7, D6 };
byte colPins[COLS] = { D5, D4, D3, D2 };
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// Circular buffer for 20 characters
char buffer[MAXBUFFERSIZE + 1];
uint8_t charCount = 0;

bool displayOn = true;

volatile bool buttonInterruptTriggered = false;
volatile unsigned long lastInterruptTime = 0;

uint8_t pressCount = 0;
unsigned long pressTimes[3] = {0};

void  buttonISR() {
  unsigned long currentTime = millis();
  if (currentTime - lastInterruptTime > DEBOUNCE_DELAY) {
    buttonInterruptTriggered = true;
    lastInterruptTime = currentTime;
  }
}

void setup() {
  pinMode(BUTTON_PIN, INPUT);
  attachInterrupt(digitalPinToInterrupt(BUTTON_PIN), buttonISR, FALLING);

  Serial.begin(9600);
  tft.begin();
  tft.fillScreen(ILI9341_BLACK);
  tft.setTextSize(2);
  tft.setCursor(3, 80);
  tft.setTextColor(ILI9341_RED);
  buffer[0] = '\0';
}

void loop() {
  if (buttonInterruptTriggered) {
    buttonInterruptTriggered = false;
    buttonHandler();
  }

  KeypadHandler();
}

void buttonHandler() {
  recordPressTime();
  toggleDisplay();
}

void recordPressTime() {
  unsigned long current = millis();
  pressTimes[pressCount % 3] = current;
  pressCount++;

  if (pressCount >= 3) {
    uint32_t delta = pressTimes[(pressCount - 1) % 3] - pressTimes[(pressCount - 3) % 3];
    if (delta <= 3000) {
      clearDisplay();
      pressCount = 0; 
    }
  }
}

void toggleDisplay() {
  displayOn = !displayOn;
  if (displayOn) {
    refreshDisplay();
  } else {
    tft.fillScreen(ILI9341_BLACK);
  }
}

void KeypadHandler() {
  char key = keypad.getKey();
  if (key) {
    updateBuffer(key);
    if (displayOn) {
      refreshDisplay();
    }
  }
}

void updateBuffer(char key) {   
  if (charCount < MAXBUFFERSIZE) {
    buffer[charCount++] = key;
  } else {
    for (int i = 0; i < MAXBUFFERSIZE - 1; ++i) {
      buffer[i] = buffer[i + 1];
    }
    buffer[MAXBUFFERSIZE - 1] = key;
  }
  buffer[charCount] = '\0';
}

void refreshDisplay() {
  tft.fillRect(0, 120, 320, 30, ILI9341_BLACK);
  tft.setCursor(0, 120);
  tft.print(buffer);
}

void clearDisplay() {
  charCount = 0;
  buffer[0] = '\0';
  if (displayOn) {
    tft.fillScreen(ILI9341_BLACK);
  }
}
