 Phase 1: Deploy IoT Sensors

# Acquire Hardware:
  Purchase the required hardware, cluding Raspberry Pi devices, ultrasonic sensors , jumper wires, a power supply, and an internet connection for the Raspberry Pi.
# Set Up Raspberry Pi:
   Install Raspbian OS on each Raspberry Pi and configure them for remote access (SSH) if necessary.
   
# Wiring Ultrasonic Sensors:
    Connect the ultrasonic sensors to the Raspberry Pi. The connections typically involve VCC, Trig, Echo, and GND pins.
# Write Python Code:
   Develop Python code on each Raspberry Pi to read data from the ultrasonic sensors. You can use the RPi. The code should measure distance and transmit the data to a central server or database.

   #  Python code for ultrasonic sensor on Raspberry Pi
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
TRIG_PIN = 23
ECHO_PIN = 24

GPIO.setup(TRIG_PIN, GPIO.OUT)
GPIO.setup(ECHO_PIN, GPIO.IN)

try:
    while True:
        # Measure distance here and send data to the central server or database
        time.sleep(1)

except KeyboardInterrupt:
    GPIO.cleanup()

# Phase 2: Develop the Transit Information Platform

# Set Up Central Server:
    Create a central server to receive, process, and store data from the IoT sensors. 
# Database Setup:
    Set up a database to store parking availability data. Popular choices include MySQL, PostgreSQL, or NoSQL databases like MongoDB.
# API Development:
    Develop APIs on the central server to receive data from the Raspberry Pi devices.

# Sample Flask API endpoint for data submission
from flask import Flask, request
import sqlite3

app = Flask(__name__)

@app.route('/submit_data', methods=['POST'])
def submit_data():
    data = request.get_json()
    # Store data in the database
    return 'Data received and stored.'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

# Database Integration:
     Integrate the database with your APIs so that data received from IoT sensors is stored correctly.
# Phase 3: Develop the Mobile App
# Install Necessary Tools:
     Install the necessary development tools on your computer, including Python, Kivy.
# UI Design:
    Design the user interface for your mobile app using Kivy's declarative language.
# App Logic:
    Write Python code for the mobile app that interacts with the APIs provided by your central server. The app should request parking availability data and display it to users in real-time.

# Example Kivy-based Python Code:
      Here is a simple example of a Kivy-based Python code for a mobile app that fetches parking availability data:

from kivy.app import App
from kivy.uix.label import Label
from kivy.network.urlrequest import UrlRequest
import json

class SmartParkingApp(App):
    def build(self):
        label = Label(text='Parking Availability: Checking...')
        self.update_parking_status(label)
        return label

    def update_parking_status(self, label):
        url = 'https://your-central-server.com/api/get_parking_status'
        req = UrlRequest(url, on_success=self.on_success, on_failure=self.on_failure)
        req.label = label

    def on_success(self, req, result):
        label = req.label
        data = json.loads(result)
        label.text = f'Parking Availability: {data["availability"]}'

    def on_failure(self, req, result):
        label = req.label
        label.text = 'Failed to fetch data.'

if __name__ == '__main__':
    SmartParkingApp().run()

# Phase 4: Integration Using Python

# Mobile App Integration:
   Ensure the mobile app communicates with the central server to fetch real-time parking availability data. This involves making HTTP requests to the API endpoints you've developed.
# Monitoring and Updates:
   Continuously monitor and update the parking availability data in the mobile app. You can set up a timer or trigger updates based on user interactions.
# Testing and Deployment:
  Test the complete system to ensure that IoT sensors, the central server, and the mobile app are working together seamlessly.
Deploy the system to your desired environment, whether it's on a local network, the internet, or the cloud.
# Security and Authentication:
 Implement proper security measures and authentication mechanisms to protect the data and ensure that only authorized users can access the mobile app and API.

# Example Output - Raspberry Pi Data Transmission:

Data sent successfully: {'distance': 34.28}
Data sent successfully: {'distance': 33.95}
Data sent successfully: {'distance': 34.12}
Data sent successfully: {'distance': 33.80}

In this example, the Raspberry Pi periodically measures the distance using the ultrasonic sensor and successfully sends the data to the central server, which acknowledges the receipt of data.

# Example Output - Mobile App UI:

Assuming the Kivy-based mobile app UI displays a label that shows the parking availability status, here are example output changes:

Initial Display:
"Parking Availability: Checking..."
After Refresh Button Clicked (Data Successfully Retrieved):
"Parking Availability: Available"
After Another Refresh (Data Retrieval Failed):
"Failed to fetch data."

In the example above, the app first shows that it's checking for parking availability, then updates the status to "Available" after successful data retrieval, and finally displays a failure message when data retrieval fails.
