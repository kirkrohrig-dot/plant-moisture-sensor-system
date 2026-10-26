# Plant Moisture Sensor System

This project is my original design of a multi-plant moisture metering system using a single Seeed XIAO ESP32-C6 controller integrated with Home Assistant via Zigbee.

It supports up to 3 sensor probes, connected as satellites to a master hub using USB-C cables. The master hub contains the ESP32-C6, USB-C ports, and various resistors/capacitors to regulate and stabilize the power supply and analog voltage readings from the sensor satellites. Power is supplied via a standard USB wall adapter (5V) and USB-C cable connected the ESP32-C6's USB port.

The Home Assistant Zigbee integration exposes individual sensor voltages along with slider helpers to easily calibrate the dry (plant needs watering) and wet (plant fully watered) voltages. Once calibrated, the moisture is displayed as a percentage. These are all Entities which can be easily added to HA dashboards or third-party dashboards such as GlassHome, including the calibration sliders for easy access to make on-the-fly adjustments.

The materials needed for the project are as follows:

## MASTER HUB UNIT

1x	Seeed Studio XIAO ESP32-C6  

3x	10 kΩ resistors	(series protection/filtering for analog outputs)  
   
3x	0.1 µF ceramic capacitors	(analog input noise filtering)  
   
3x	1 MΩ resistors (pull-downs to give unplugged inputs a defined state)  
   
1x	10 µF electrolytic or ceramic capacitor  

3x	0.1 µF ceramic capacitors	(local bypass capacitor at each sensor connector)  

1x	Small perfboard  

1x	USB-C cable	(USB 2.0 minimum for firmware upload, after that a charging-only cable may be used for ongoing operation)  

1x	UL-listed 5 V USB wall adapter, 1 A or greater  

## SENSORS  

3x	MusRock analog capacitive soil-moisture sensors (https://www.amazon.com/Corrosion-Resistant-Capacitive-Moisture-Arduino/dp/B0FGD5H8X3/ref=sr_1_3?s=hi&sr=1-3)  

3x Female USB-C ports (https://www.amazon.com/dp/B0F3WS4TQS)  

3x USB-C cables (require data - USB2.0 or above)  

6x M2.5 x 6mm screws  

## Pin Assignment

| Connection | ESP32-C6 Label | ESPHome Pin |
| -------- | -------- | -------- |
| Plant 1 Signal    | D0/A0   | GPIO0  |
| Plant 2 Signal    | D1/A1   | GPIO1  |
| Plant 3 Signal    | D2/A2   | GPIO2  |
| All sensor power  | 3V3     | 3.3V   |
| All sensor ground | GND     | GND    |
| Wall power        | USB-C   | 5V USB |


## Wiring  

### Power

The complete power arrangement is:
<br>
- XIAO powered through USB-C
- XIAO 3V3 connected to all three sensor VCC terminals
- XIAO ground connected to all three sensor grounds
- One 10 µF capacitor across the 3.3 V and ground distribution rails
- One 0.1 µF capacitor across VCC and GND at each sensor connector.
<br>
Do not power the probes from the XIAO’s 5 V/VBUS pin. A probe powered at 5 V might produce an analog output above the ESP32-C6’s safe 3.3 V input level. Powering the probes from 3.3 V keeps their outputs compatible with the ADC.
<br>
### Filtering components

For each channel:
<br>
1. Connect sensor AOUT to one end of the 10 kΩ resistor
2. Connect the other end to the appropriate XIAO ADC pin
3. Connect the 0.1 µF capacitor between that ADC pin and ground
4. Connect the 1 MΩ pull-down between that same ADC pin and ground.
<br>
Place the capacitor and pull-down on the XIAO side of the 10 kΩ resistor. An unplugged port will read close to zero because of the pull-down. Firmware should therefore classify something such as below 0.10 V as disconnected, rather than interpreting it as extremely wet soil.

### ADC configuration considerations

ESPHome’s ADC component defaults to a lower voltage range unless attenuation is configured. Use attenuation: auto or verify 12db operation because your expected readings extend to approximately 2.6 V.
  
### Calibration

Use this sequence independently for all three ports:
<br>
1. Measure the unplugged value; it should be near 0 V and flagged as disconnected.
2. Record the stabilized reading in dry soil.
3. Saturate the pot fully, allow it to drain normally, and record the wet value.
4. Use 2.6 V as the dry ceiling to start. This can be adjusted if any sensors return voltage above this cap.
5. Use each probe’s measured wet reading for 100%; 0.68 V is only an initial reference and can also be adjusted based on individual readings.
6. Clamp the final result to 0–100%.
<br>
Because the sensor relationship is reversed, the basic calculation is:
![Alt text](img/Formula.jpg)
![Alt text](img/Formula.jpg)
Apply the disconnected check before this formula.

