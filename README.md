// Blynk Credentials
#define BLYNK_TEMPLATE_ID "TMPL3pSSGNsyl"
#define BLYNK_TEMPLATE_NAME "Cry Baby Detection"
#define BLYNK_AUTH_TOKEN "4jELiAqnoOlr9duIVvsQmHwTFIpg3cuQ"


#include <ESP32Servo.h>          // Use this instead of Servo.h
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>


// WiFi Credentials
char ssid[] = "ERROR";       // <-- Replace with your WiFi
char pass[] = "skansal937";   // <-- Replace with your WiFi password


// Pins
#define SOUND_SENSOR_PIN 34    // Sound sensor connected to GPIO34
#define SERVO_PIN 27           // Servo connected to GPIO27


Servo myServo;  // Create a servo object
bool notified = false;  // To avoid sending too many notifications
void setup() {
  Serial.begin(115200);          // Start Serial
  myServo.attach(SERVO_PIN);     // Attach the servo
  myServo.write(0);              // Start at 0 degrees
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);  // Connect to Blynk
}
void loop() {
  Blynk.run();  // Keep Blynk connection alive
 int soundValue = analogRead(SOUND_SENSOR_PIN);
  Serial.print("Sound Sensor Value: ");
  Serial.println(soundValue);
 if (soundValue > 2000) {    // Threshold for sound detection
    if (!notified) {
      Blynk.logEvent("cr", "Loud sound detected!");  // Send Notification
      notified = true;  // Mark as notified
    }
    // Move servo 5 times
    for (int i = 0; i < 5; i++) {
      myServo.write(90);  // Move to 90 degrees
      delay(500);         // Wait
      myServo.write(0);   // Back to 0 degrees
      delay(500);         // Wait
    }
  } else {
    notified = false;  // Reset notification flag when sound is low
  }
  delay(100); // Small delay before next reading
}
