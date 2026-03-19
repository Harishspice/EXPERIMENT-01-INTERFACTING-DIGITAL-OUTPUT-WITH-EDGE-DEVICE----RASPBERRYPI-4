# EXPERIMENT-01-INTERFACTING-DIGITAL-OUTPUT-WITH-EDGE-DEVICE---(RASPBERRYPI-PI4)
### NAME : HARISH R
### DEPARTMENT : CSE(IOT)
### ROLL NO : 212222110012
### DATE OF EXPERIMENT : 19.3.26

### AIM
To interface a digital output device (LED) with the Raspberry Pi 4 and control it using Python.

## APPARATUS REQUIRED
Raspberry Pi 4
LED (Light Emitting Diode)
330Ω Resistor
IR Sensor
Breadboard
Jumper Wires
USB Cable
 ## THEORY

![Raspberry Pi Pin](https://github.com/user-attachments/assets/19e5a1e7-cb46-4909-ba59-e4f4560cae03)





 
 
 
 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM 


The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.


## Working Principle:
Experiment 1A
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to GP15 via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).

Experiment 1B
The LED is connected to one of the GPIO pins of the Raspberry Pi 4.
The IR sensor is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the GPIO pin HIGH to turn the LED ON and LOW to turn it OFF based on the IR sensor.
CIRCUIT DIAGRAM
Connect the anode (longer leg) of the LED to any one GPIO via a 330Ω resistor.
Connect the cathode (shorter leg) of the LED to GND (ground).
Connect the IR sensor Vcc to any +5V.
Connect the IR sensor GND to any GND.
Connect the IR sensor OUT to any one GPIO. 

## PROGRAM (Python)
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "6D98N0MXJUBXA8L8"
CHANNEL_ID = 3249454
THINGSPEAK_URL = "https://api.thingspeak.com/update"

# Set GPIO numbering mode
GPIO.setmode(GPIO.BCM)

# Define LED pin
LED_PIN = 18

# Set GPIO18 as output
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=6D98N0MXJUBXA8L8&field1={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)

try:
    while True:
        # LED ON
        GPIO.output(LED_PIN, GPIO.HIGH)
        print("LED ON")
        send_to_thingspeak(1)
        time.sleep(15)

        # LED OFF
        GPIO.output(LED_PIN, GPIO.LOW)
        print("LED OFF")
        send_to_thingspeak(0)
        time.sleep(15)

except KeyboardInterrupt:
    print("Program stopped")

finally:
    GPIO.cleanup()


````

### OUPUT  

### Experiment 1A

### LED OFF

<img width="1600" height="1297" alt="image" src="https://github.com/user-attachments/assets/2c6cea5c-9973-419c-97c3-7caf612024ab" />



<img width="808" height="1280" alt="image" src="https://github.com/user-attachments/assets/0356cb69-05a1-4bf3-b837-1f86c3e7e87e" />




<img width="808" height="1280" alt="image" src="https://github.com/user-attachments/assets/6fd35b5c-da25-4cdd-b054-a2ec49dab044" />

### LED ON: 


<img width="1600" height="1430" alt="image" src="https://github.com/user-attachments/assets/5e0995f6-1757-4275-9226-0e17a932c5ad" />



<img width="861" height="1280" alt="image" src="https://github.com/user-attachments/assets/8ad1b850-1376-4d5f-a17c-bc34e9dc7d12" />




<img width="1905" height="918" alt="Screenshot 2026-02-04 111629" src="https://github.com/user-attachments/assets/a5c15b2e-aa5d-467b-8fc2-ead4935dbcaa" />


Experiment 1B

### PROGRAM(python)
```
import RPi.GPIO as GPIO
import time
import urllib.request

# ThingSpeak details
WRITE_API_KEY = "6D98N0MXJUBXA8L8"
CHANNEL_ID =  3249454
THINGSPEAK_URL = "https://api.thingspeak.com/update"



# Pin setup
SENSOR_PIN = 23   # Input from sensor
LED_PIN = 18      # Output to LED

# GPIO mode
GPIO.setmode(GPIO.BCM)

# Setup pins
GPIO.setup(SENSOR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)

def send_to_thingspeak(value):
    url = f"https://api.thingspeak.com/update?api_key=6D98N0MXJUBXA8L8&field2={value}"
    urllib.request.urlopen(url)
    print("Sent to ThingSpeak:", value)


print("Sensor + LED system running...")

try:
    while True:
        sensor_value = GPIO.input(SENSOR_PIN)

        if sensor_value == 0:   # Many IR sensors give LOW when object detected
            print("Object Detected! LED ON")
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_to_thingspeak(1)

            time.sleep(15)
        else:
            print("No Object. LED OFF")
            GPIO.output(LED_PIN, GPIO.LOW)
            send_to_thingspeak(0)

            time.sleep(15)

        time.sleep(0.1)

except KeyboardInterrupt:
    print("Stopped by user")

finally:
    GPIO.cleanup()
```

# LED OFF(NO OBJECT DETECTED):


<img width="1201" height="1600" alt="image" src="https://github.com/user-attachments/assets/469fe163-9b7a-418b-84b2-ad3e0c97c552" />



<img width="680" height="1280" alt="image" src="https://github.com/user-attachments/assets/734387d8-67c3-4fcb-917c-ce08ad266ca1" />



<img width="1893" height="922" alt="Screenshot 2026-02-04 120121" src="https://github.com/user-attachments/assets/7e223307-98bf-428c-9179-a5fd62222f9a" />



# LED ON(OBJECT DETECTED):


<img width="1430" height="1600" alt="image" src="https://github.com/user-attachments/assets/1ef8aac9-950f-4d0d-a86d-8bf7dad83435" />




<img width="730" height="1280" alt="image" src="https://github.com/user-attachments/assets/3fcf6378-cc1d-4826-a6c8-a657a8dedc97" />




<img width="1887" height="910" alt="Screenshot 2026-02-04 120138" src="https://github.com/user-attachments/assets/c41af1d7-53c3-4fb7-89fa-6aeae0a49918" />

 
## RESULTS
The LED connected to the Raspberry Pi 4 successfully turns ON and OFF at  user defined time  confirming the proper interfacing of a digital output.
