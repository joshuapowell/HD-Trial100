# BME680
Temperature Sensor

https://acrobotic.com/products/brk-00019

## Specification

- Board Size: 18mm × 16.3mm
- Digital interface: I2C (up to 3.4 MHz) and SPI (3 and 4 wire, up to 10 MHz)
- Supply voltage: 3~5V
- Pinout: VCC, GND, SCL/SCK, SDA/SDI, SDO, CS (active low)
- Operating voltage:  1.71~3.6 V (Vdd) and 1.2~3.6V (Vddio)
- Current consumption: 0.09~12mA for p/h/T/gas depending on operation mode; 0.15 μA in sleep mode
- Operating range: –40~85°C, 0~100%r.H., 300~1100hPa
- Individual humidity, pressure and gas sensors can be independently enabled/disabled

<img src="https://github.com/joshuapowell/HD-Trial100/blob/master/trials/temperature/BMP680/images/ai_brk00019_iso1_867cd8a8-321d-459b-b864-e0a3423c0305.jpg?raw=true" />

## Wire Up

- https://github.com/adafruit/Adafruit_Sensor
- https://github.com/adafruit/Adafruit_BME680
- https://learn.adafruit.com/adafruit-bme680-humidity-temperature-barometic-pressure-voc-gas/arduino-wiring-test

## Manufacturer Information
- https://learn.acrobotic.com/datasheets/BME680.pdf
- https://www.bosch-sensortec.com/bst/products/all_products/bme680
- https://github.com/BoschSensortec/BME680_driver

## Understanding VOC
- https://www.epa.gov/indoor-air-quality-iaq/technical-overview-volatile-organic-compounds

## Example Code
http://www.theorycircuit.com/bme680-arduino/

```
/*************************************************************************** This is a library for the BME680 gas, humidity, temperature & pressure sensor Designed specifically to work with the Adafruit BME680 Breakout
  ----> http://www.adafruit.com/products/3660     These sensors use I2C or SPI to communicate, 2 or 4 pins are required to interface. Adafruit invests time and resources providing this open source code,  Written by Limor Fried & Kevin Townsend for Adafruit Industries.
  BSD license, all text above must be included in any redistribution
 ***************************************************************************/

#include <Wire.h>
#include <SPI.h>
#include <Adafruit_Sensor.h>
#include "Adafruit_BME680.h"

#define BME_SCK 13
#define BME_MISO 12
#define BME_MOSI 11
#define BME_CS 10

#define SEALEVELPRESSURE_HPA (1013.25)

Adafruit_BME680 bme; // I2C
//Adafruit_BME680 bme(BME_CS); // hardware SPI
//Adafruit_BME680 bme(BME_CS, BME_MOSI, BME_MISO,  BME_SCK);

void setup() {
  Serial.begin(9600);
  while (!Serial);
  Serial.println(F("BME680 test"));

  if (!bme.begin()) {
    Serial.println("Could not find a valid BME680 sensor, check wiring!");
    while (1);
  }

  // Set up oversampling and filter initialization
  bme.setTemperatureOversampling(BME680_OS_8X);
  bme.setHumidityOversampling(BME680_OS_2X);
  bme.setPressureOversampling(BME680_OS_4X);
  bme.setIIRFilterSize(BME680_FILTER_SIZE_3);
  bme.setGasHeater(320, 150); // 320*C for 150 ms
}

void loop() {
  if (! bme.performReading()) {
    Serial.println("Failed to perform reading :(");
    return;
  }
  Serial.print("Temperature = ");
  Serial.print(bme.temperature);
  Serial.println(" *C");

  Serial.print("Pressure = ");
  Serial.print(bme.pressure / 100.0);
  Serial.println(" hPa");

  Serial.print("Humidity = ");
  Serial.print(bme.humidity);
  Serial.println(" %");

  Serial.print("Gas = ");
  Serial.print(bme.gas_resistance / 1000.0);
  Serial.println(" KOhms");

  Serial.print("Approx. Altitude = ");
  Serial.print(bme.readAltitude(SEALEVELPRESSURE_HPA));
  Serial.println(" m");

  Serial.println();
  delay(2000);
}
```
