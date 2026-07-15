
# One-handed-video-game-controller
An accessible, ergonomic one-handed game controller built with Arduino Leonardo. 
Designed for individuals with limited hand mobility, it connects to a PC via USB 
and works as a plug-and-play HID device — no drivers needed!!

## Features
- Joystick mapped to mouse movement
- 4 face buttons mapped to keyboard inputs
- USB HID — plug and play, no additional software needed
- Designed for single hand use

## Components
- Arduino Leonardo
- Analog Joystick Module
- 4x Push Buttons
- Breadboard and jumper wires

## Circuit
<img width="1279" height="707" alt="Screenshot 2026-07-15 001621" src="https://github.com/user-attachments/assets/d6e6e976-4e63-4830-a63f-cbb756e05da9" />

## How to Use
1. Upload the code to Arduino Leonardo
2. Plug into PC via USB
3. Controller is automatically recognized as HID device
4. Open any game and play!!

## Code
Built using Arduino IDE with Keyboard.h and Mouse.h libraries
```cpp
#include <Keyboard.h>
#include <Mouse.h>

const int inX = A0; // analog input for x-axis
const int inY = A1; // analog input for y-axis
const int butJoy = 2; // input for detecting whether the joystick/button is pressed

const int butBlue = 4; 
const int butYellow = 6; 
const int butRed = 3;
const int butGreen = 5; 
bool blueState; 
bool yellowState;
bool redState;
bool greenState;

int xValue = 0; // variable to store x value
int yValue = 0; // variable to store y value
bool joyState; // variable to store the button's state => 1 if not pressed

/* parameters for joystick
int range = 12;
int threshold = (range/4);
int center = (range/2);
*/

void setup() {
  pinMode(inX, INPUT); // setup x input
  pinMode(inY, INPUT); // setup y input
  pinMode(butJoy, INPUT_PULLUP); // we use a pullup-resistor for the button functionality
  pinMode(butBlue, INPUT_PULLUP);
  pinMode(butYellow, INPUT_PULLUP);
  pinMode(butRed, INPUT_PULLUP);
  pinMode(butGreen, INPUT_PULLUP);
  Serial.begin(9600); // Setup serial connection for print out to console

  Keyboard.begin();
  Mouse.begin();
  
}

void loop() {
  // joystick

  xValue = analogRead(inX); // reading x value [range 0 -> 1023]
  yValue = analogRead(inY); // reading y value [range 0 -> 1023]
  joyState = digitalRead(butJoy); // reading button state: 1 = not pressed, 0 = pressed

if (joyState == 1) { 
  xValue = map(xValue, 0, 1023, -10, 10);  // using 0 and 1023 will make the cursor fly across
  yValue = map(yValue, 0, 1023, -10, 10); // the screen so we will "map" those value to -10 and 10
  Mouse.move(xValue,yValue,0);                // analogRead() gives values from 0 -1023 but Mouse.move expects 
}                                             // ranges like -10 -> 10 or -5 -> 5

xValue = 0;
yValue = 0;

// blue & green corresponding to space & e
blueState = digitalRead(butBlue);
if (blueState == 1) {
  Keyboard.press('1');
}
else {
  Keyboard.release('1');
}
blueState = false;

greenState = digitalRead(butGreen);
if (greenState == 1) {
  Keyboard.press('e');
}
else {
  Keyboard.release('e');
}
greenState = false;

// yellow & red corresponding to left and right click
yellowState = digitalRead(butYellow);
if (yellowState == 1) {
  Mouse.press(MOUSE_LEFT);
}
else {
  Mouse.release(MOUSE_LEFT);
}
yellowState = false;

redState = digitalRead(butRed); // was greenState in place of redState
if (redState == 1) {
  Mouse.press(MOUSE_RIGHT);
}
else {
  Mouse.release(MOUSE_RIGHT);
}
redState = false;

  
  // The following delay of 1000ms is only for debugging reasons (it's easier to follow the values on the serial monitor)
  delay(5); // Probably not needed for most applications
}
```

## Demo
[Watch live Demo on YouTube!!](https://www.youtube.com/shorts/mtXVW7fpkCw)

## Media
<img width="1600" height="1200" alt="one handed video game controller" src="https://github.com/user-attachments/assets/d8f6c4d4-eecf-4659-99d6-1794210594e0" />

