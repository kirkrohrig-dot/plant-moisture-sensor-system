# plant-moisture-sensor-system
Plant Moisture Sensor System

This project is my original design of a multi-plant moisture metering system using a single Seeed XIAO ESP32-C6 controller integrated with Home Assistant via Zigbee.

It supports up to 3 sensor probes, connected as satellites to a master hub using USB-C cables. The master hub contains the ESP32-C6, USB-C ports, and various resistors/capacitors to regulate and stabilize the power supply and analog voltage readings from the sensor satellites. Power is supplied via a standard USB wall adapter (5V) and USB-C cable connected the ESP32-C6's USB port.

The Home Assistant Zigbee integration exposes individual sensor voltages along with slider helpers to easily calibrate the dry (plant needs watering) and wet (plant fully watered) voltages. Once calibrated, the moisture is displayed as a percentage. These are all Entities which can be easily added to HA dashboards or third-party dashboards such as GlassHome, including the calibration sliders for easy access to make on-the-fly adjustments.

The materials needed for the project are as follows:

MASTER HUB UNIT

1x	Seeed Studio XIAO ESP32-C6

3x	10 kΩ resistors	(series protection/filtering for analog outputs)
   
3x	0.1 µF ceramic capacitors	(analog input noise filtering)
   
3x	1 MΩ resistors (pull-downs to give unplugged inputs a defined state)
   
1x	10 µF electrolytic or ceramic capacitor	

3x	0.1 µF ceramic capacitors	(local bypass capacitor at each sensor connector)

1x	Small perfboard 

1x	USB-C cable	(USB 2.0 minimum for firmware upload, after that a charging-only cable may be used for ongoing operation)

1x	UL-listed 5 V USB wall adapter, 1 A or greater

SENSORS

3x	MusRock analog capacitive soil-moisture sensors (https://www.amazon.com/Corrosion-Resistant-Capacitive-Moisture-Arduino/dp/B0FGD5H8X3/ref=sr_1_3?s=hi&sr=1-3)

3x Female USB-C ports (https://www.amazon.com/dp/B0F3WS4TQS)

3x USB-C cables (require data - USB2.0 or above)

6x M2.5 x 6mm screws

