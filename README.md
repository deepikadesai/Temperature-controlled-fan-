# Temperature-controlled-fan-
#include <DHT.h>

// Pin Definitions
#define DHTPIN 15    // Pin for DHT11
#define DHTTYPE DHT11
#define IN1 16       // Motor Driver IN1 pin
#define IN2 17       // Motor Driver IN2 pin

#define MOTOR_ON_TEMP 30 // Temperature threshold to start motor (30°C)

DHT dht(DHTPIN, DHTTYPE);  // Initialize DHT sensor

void setup() {
  // Start serial communication
  Serial.begin(115200);
  
  // Initialize DHT sensor
  dht.begin();

  // Set motor control pins as output
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
}

void loop() {
  // Read temperature from DHT11
  float temperature = dht.readTemperature();
  
  // Check if the reading is valid
  if (isnan(temperature)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  
  // Print the temperature to Serial Monitor
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println("°C");
  
  // Check if temperature is below the threshold to turn on the motor
  if (temperature < MOTOR_ON_TEMP) {
    // Turn motor ON (IN1 High, IN2 Low)
    digitalWrite(IN1, HIGH);
    digitalWrite(IN2, LOW);
    Serial.println("Motor ON");
  } else {
    // Turn motor OFF (IN1 Low, IN2 Low)
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);
    Serial.println("Motor OFF");
  }

  // Wait before checking again
  delay(2000);  // Delay for 2 seconds
}
