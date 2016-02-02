//load libraries
#include <VirtualWire.h>
#include <Wire.h>
#include <LCD.h>
#include <LiquidCrystal_I2C.h>

//Initialise the LCD
LiquidCrystal_I2C  lcd(0x27, 2, 1, 0, 4, 5, 6, 7); // 0x27 is the I2C bus address for an unmodified backpack

int i;    //and integer used to count

boolean isSignal = 1; //1=yes 0=no

int rain = 0;
int rainVal = 0;

String rainState[] = {"Not Raining", "Raining", "Heavy Rain"};

long sigDown = 0; //time until no signal is displayed on screen after signal loss
long sigDownCount = 20000; //same as sigDown

void setup() {
  Serial.begin(9600);

  lcd.begin (16, 2); // for 16 x 2 LCD module
  lcd.setBacklightPin(3, POSITIVE);
  lcd.setBacklight(HIGH);

  pinMode(12, OUTPUT);

  //Define the receiver pin and rate
  vw_set_rx_pin(12);       //Sets pin D12 as the RX Pin
  vw_setup(2000);       // Bits per sec
  vw_rx_start();           // Start the receiver PLL running

  lcd.home (); // set cursor to 0,0
  lcd.print("Weather Monitor");
  lcd.setCursor (0, 1);       // go to start of 2nd line
  lcd.print("Receiver V1.0");
  delay(2000);
  lcd.clear();
  lcd.home (); // set cursor to 0,0
  lcd.print("By");
  lcd.setCursor (0, 1);       // go to start of 2nd line
  lcd.print("Owain Jones");
  delay(2000);
}

void loop() {

  uint8_t buf[VW_MAX_MESSAGE_LEN];
  uint8_t buflen = VW_MAX_MESSAGE_LEN;
  lcd.setCursor(0, 0);

  if ( vw_get_message(buf, &buflen) ) {

    digitalWrite(13, HIGH);

    rain = buf[10];

    rainCode();

    Serial.println("===received ints===");
    Serial.print("Temp: ");
    Serial.write(buf[2]);
    Serial.write(buf[3]);
    Serial.print("  Humidity: ");
    Serial.write(buf[6]);
    Serial.write(buf[7]);
    Serial.print("  Rain number: ");
    Serial.write(rain);
    Serial.print("  Rain ascii: ");
    Serial.print(rain);
    Serial.print("  RainVal: ");
    Serial.write(rainVal);
    Serial.print("  Rain state:  ");
    Serial.println(rainState[rainVal]);
    Serial.println("==========");

    lcd.clear();
    lcd.home (); // set cursor to 0,0
    lcd.print("Temp:");
    lcd.write(buf[2]);
    lcd.write(buf[3]);
    lcd.print(" Humid:");
    lcd.write(buf[6]);
    lcd.write(buf[7]);
    lcd.setCursor (0, 1);       // go to start of 2nd line
    lcd.print("Rain:");
    lcd.print(rainState[rainVal]);

    sigDown = sigDownCount;
    isSignal = 1;

  }

  else {
    digitalWrite(13, LOW);
    sigDown--;

    Serial.print("sigDown");
    Serial.println(sigDown);

    if (sigDown < 0) {
      if (isSignal == 1) {
        lcd.clear();
        isSignal = 0;
      }
      lcd.print("NO SIGNAL");
      lcd.setCursor(0, 1);
      lcd.print(sigDown);

      Serial.println("NO SIGNAL");

      delay(500);
    }

  }
}
