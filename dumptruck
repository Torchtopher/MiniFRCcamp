#include <Alfredo_NoU2.h>
#include <AlfredoConnect.h>
#include <BluetoothSerial.h>

NoU_Motor fleftMotor(3);
NoU_Motor frightMotor(2);
NoU_Motor bleftMotor(4);
NoU_Motor brightMotor(1);

NoU_Servo basketServo(1);

NoU_Drivetrain drivetrain(&fleftMotor, &frightMotor, &bleftMotor, &brightMotor);

BluetoothSerial bluetooth;
bool HAS_RUN_AUTO = false;

void setup() {
bluetooth.begin("GoofyMonkey");
AlfredoConnect.begin(bluetooth);
bluetooth.println("Starting motor and servo test program.");
}

bool extendBasket = false;
int basketIdx = 18;
unsigned long lastBasketTime = 0;

void set_all_motors(double speed) {
  fleftMotor.set(speed);
  frightMotor.set(speed);
  bleftMotor.set(speed * -1);
  brightMotor.set(speed * -1);
}

void run_auto(double multiplier) {
  HAS_RUN_AUTO = true;
  unsigned long startTime = millis();

  bluetooth.println("Running auto!");

  while ((millis() - startTime) < 850)  {
    set_all_motors(1.0);
  }
  set_all_motors(0.0);
}

void loop() {
    float throttle = 0;
    float rotation = 0;

  if (AlfredoConnect.keyHeld(Key::F)) {
    extendBasket = true;
    lastBasketTime = millis();
  }
  
  if (AlfredoConnect.keyHeld(Key::G)) {
    extendBasket = false;
    basketServo.write(180);
  }
  
  if (extendBasket && (millis() - lastBasketTime) > 100) {
    basketServo.write(basketIdx * 10);
    bluetooth.println("Writing to servo");
    lastBasketTime = millis();
    basketIdx--;
    if (basketIdx < 0) {
      basketIdx = 18;
      extendBasket = false;
    } 
  }
  
  if (AlfredoConnect.keyHeld(Key::L)) {
    run_auto(1);
  }

    // Set the throttle of the robot based on what key is pressed
    if (AlfredoConnect.keyHeld(Key::A)) {
        throttle = 1;
    }
    else if (AlfredoConnect.keyHeld(Key::D)) {
        throttle = -1;
    }
    // Set which direction the robot should turn based on what key is pressed
    if (AlfredoConnect.keyHeld(Key::W)) {
        rotation = -1;
    }
    else if (AlfredoConnect.keyHeld(Key::S)) {
        rotation = 1;
    }

    // Make the robot drive
    drivetrain.curvatureDrive(throttle, rotation);

    AlfredoConnect.update();
}
