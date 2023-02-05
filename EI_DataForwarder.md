# Data forwarder

The data forwarder is used to easily relay data from any device to Edge Impulse over serial. Devices write sensor values over a serial connection, and the data forwarder collects the data, signs the data and sends the data to the ingestion service. 

More details: [Data forwarder](https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-data-forwarder)


## Trying to collect Analog Data from AMB23 to EI 

![](https://amebaiotdocuments.readthedocs.io/en/latest/_images/image285.png)

## AM23 <> MPU6050 



```
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>
#include <Wire.h>

#define CONVERT_G_TO_MS2    9.80665f
#define FREQUENCY_HZ        20
#define INTERVAL_MS         (1000 / (FREQUENCY_HZ + 1))

Adafruit_MPU6050 mpu;

static unsigned long last_interval_ms = 0;

void setup() {
  Serial.begin(115200);
  //Serial.println("Started");

  if (!mpu.begin()) {
    //  Serial.println("Failed to initialize IMU!");
    while (1);
    delay(10);
  }

  mpu.setAccelerometerRange(MPU6050_RANGE_8_G);

  mpu.setGyroRange(MPU6050_RANGE_500_DEG);

  mpu.setFilterBandwidth(MPU6050_BAND_21_HZ);

  mpu.setCycleRate(MPU6050_CYCLE_20_HZ);

  delay(100);
}

void loop() {

  
  /* Get new sensor events with the readings */
  if (millis() > last_interval_ms + INTERVAL_MS) {
    last_interval_ms = millis();
    sensors_event_t a, g, temp;
    mpu.getEvent(&a, &g, &temp);
    

    /* Print out the values */
    // Serial.print("Acceleration X: ");
    Serial.print(a.acceleration.x * CONVERT_G_TO_MS2);
    Serial.print('\t');
    
    Serial.print(a.acceleration.y * CONVERT_G_TO_MS2);
    Serial.print('\t');
 
    Serial.print(a.acceleration.z * CONVERT_G_TO_MS2);
    Serial.print('\t');
  

    Serial.print(g.gyro.x);
    Serial.print('\t');

    Serial.print(g.gyro.y);
    Serial.print('\t');
 
    Serial.println(g.gyro.z);
  
  }
}
```






