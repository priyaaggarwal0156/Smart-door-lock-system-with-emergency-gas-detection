# Smart-door-lock-system-with-emergency-gas-detection
This repository consist of code of 'Smart door lock system with emergency gas detection' project
the code for this project is as given below:-
#include <Keypad.h>
#include <Servo.h>

Servo myServo;

// -------- KEYPAD SETUP --------
const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// -------- PIN DEFINITIONS --------
int servoPin = 10;
int buzzerPin = 11;

// -------- PASSWORD --------
String correctPassword = "1234";
String enteredPassword = "";

void setup() {
  myServo.attach(servoPin);
  myServo.write(0);   // Locked position

  pinMode(buzzerPin, OUTPUT);
}

void loop() {

  char key = keypad.getKey();

  if (key) {

    if (key == '#') {
      checkPassword();
    }
    else if (key == '*') {
      enteredPassword = "";   // Clear input
    }
    else {
      enteredPassword += key;
    }
  }
}

// -------- CHECK PASSWORD --------
void checkPassword() {

  if (enteredPassword == correctPassword) {
    unlockDoor();
  }
  else {
    wrongPassword();
  }

  enteredPassword = "";
}

// -------- UNLOCK FUNCTION --------
void unlockDoor() {

  // 1 long beep
  digitalWrite(buzzerPin, HIGH);
  delay(800);
  digitalWrite(buzzerPin, LOW);

  myServo.write(90);   // Unlock
  delay(5000);         // Keep open 5 sec
  myServo.write(0);    // Lock again
}

// -------- WRONG PASSWORD --------
void wrongPassword() {

  // 3 short beeps
  for (int i = 0; i < 3; i++) {
    digitalWrite(buzzerPin, HIGH);
    delay(200);
    digitalWrite(buzzerPin, LOW);
    delay(200);
  }
}
