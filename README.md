1. This system logs temperature and humidity from a DHT11 sensor to an SD card every 1 minutes using an Arduino Nano.
o 
Reads temperature/humidity when triggered.
o Sends digital data to Arduino's D2 pin customizable for environmental monitoring.
2. Arduino Nano
o Uses the DHT library to decode sensor data.
o Tracks time using millis() (non-blocking delay).
o Every 5 minutes, it:
 Reads fresh data from DHT11.
 Opens datalog.txt on the SD card.
 Appends new data in this format:
plaintext
Copy
Download
Time: [seconds] | Temp: [value]°C | Hum: [value]%
3. SD Card Module
o Communicates via SPI (pins D10-D13).
o Stores data reliably even if power is lost.
________________________________________
3. Critical Code Logic
A. Time Management
• Uses millis() (not delay) to avoid freezing the Arduino.
• Checks if 1 minutes (60,000 ms) have passed before logging again.
B. Error Handling
• SD Card Fail: Stops if SD card isn’t detected (while(1)).
• DHT11 Fail: Skips logging if sensor readings fail (isnan() check).
C. File Writing
• Opens file in FILE_WRITE mode (appends data without deleting old entries).
• Closes the file after writing to ensure data is saved.
________________________________________
4. Wiring Summary
Pin Connection
D2 DHT11 Data (White wire)
D10 SD Card CS
D11 SD Card MOSI
D12 SD Card MISO
D13 SD Card SCK
5V/GND Powers both DHT11 & SD module
________________________________________
5. Expected Output
Serial Monitor
plaintext
Copy
Download
SD Card ready.  
Read: Temp=25.0°C, Hum=50%  
Writing to SD...  
Write successful.  
[5 minutes later]  
Read: Temp=26.0°C, Hum=48%  
Writing to SD...  
Write successful.  
SD Card File (datalog.txt)
plaintext
Copy
Download
Time: 300s | Temp: 25.0°C | Hum: 50%  
Time: 600s | Temp: 26.0°C | Hum: 48%  
________________________________________
6. Key Optimizations
• Non-blocking delay: Uses millis() so Arduino can do other tasks.
• Low power: Add sleep mode between readings for battery-powered use.
• CSV format: Change to Time,Temp,Hum for Excel/Python analysis.
________________________________________
7. Troubleshooting Cheat Sheet
Issue Solution
No SD card detected Check CS pin (usually D10), reformat SD card.
DHT11 readings fail Add 4.7KΩ pull-up resistor on data pin.
Data not saved Ensure dataFile.close() is called.
Duplicate entries Use SD.remove() before writing.
