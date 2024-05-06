#include <BH1750.h>
#include <Wire.h>
#include <WiFiNINA.h> // Include the WiFi library for Arduino Nano 33 IoT

const char* ssid = "iPhone";
const char* password = "milad0404";
const char* iftttWebhookKey = "lLKWfpkA8gZdCzIbtQuf_tNhvbr4iWdezk5wsSMNEe9";

BH1750 lightMeter;

void setup() {
  Serial.begin(9600);
  while (!Serial) {
    ; // Wait for serial port to connect
  }
  
  // Initialize the I2C bus
  Wire.begin();
  
  // Initialize the BH1750 sensor
  lightMeter.begin();
  
  // Connect to Wi-Fi
  connectWiFi();
  
  Serial.println(F("BH1750 Test begin"));
}

void loop() {
  // Read light level
  float lux = lightMeter.readLightLevel();
  Serial.print("Light: ");
  Serial.print(lux);
  Serial.println(" lx");

  // Check if sunlight exposure exceeds threshold
  if (lux >= 20) { // Adjust the threshold as needed
    triggerIFTTTEvent("light_measure");
  } else if (lux == 0) { // If light intensity is 0 (no light) send event
    triggerIFTTTEvent("no_light_measure");
  }
  
  delay(10000); // Wait for 10 seconds before the next reading
}

void connectWiFi() {
  Serial.print("Connecting to Wi-Fi");
  while (WiFi.begin(ssid, password) != WL_CONNECTED) {
    Serial.print(".");
    delay(1000);
  }
  Serial.println("Connected to Wi-Fi");
}

void triggerIFTTTEvent(const char* event) {
  // Send HTTP POST request to IFTTT webhook
  WiFiClient client;
  if (client.connect("maker.ifttt.com", 80)) {
    client.print("POST /trigger/");
    client.print(event);
    client.print("/with/key/");
    client.print(iftttWebhookKey);
    client.println(" HTTP/1.1");
    client.println("Host: maker.ifttt.com");
    client.println("Connection: close\r\n");
    client.stop();
  }
}
