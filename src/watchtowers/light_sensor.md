(light-sensor-assembly)=
# Light sensor assembly

These instructions will show you how to mount the Adafruit TCS34725 sensor on a watchtower and how to correctly wire it to the Raspberry Pi.

## Requirements

You will need the following components:

```{figure} ../_images/watchtower/light-sensor/sensor_1.jpg
:figwidth: 25em
:name: fig:wt_sensor1
:align: center

Pinheaders
```

```{figure} ../_images/watchtower/light-sensor/sensor_2.jpg
:figwidth: 25em
:name: fig:wt_sensor2
:align: center

Jumper Cables
```

```{figure} ../_images/watchtower/light-sensor/sensor_3.jpg
:figwidth: 25em
:name: fig:wt_sensor3
:align: center

The sensor
```

```{figure} ../_images/watchtower/light-sensor/sensor_4.jpg
:figwidth: 25em
:name: fig:wt_sensor4
:align: center

Screws
```

```{figure} ../_images/watchtower/light-sensor/sensor_5.jpg
:figwidth: 25em
:name: fig:wt_sensor5
:align: center

Watchtower top plate
```

```{note}
The upper plate of the watchtower need appropriate holes to fit the sensor.
```

## 1 - Soldering

The first thing to do is to solder the pin headers to the actual sensor.

```{tip}
The easiest way to solder the pin headers is to use a breadboard and cut a few pin headers in order to get a 90-degree angle between the sensor and the pins.
```

```{figure} ../_images/watchtower/light-sensor/sensor_6.jpg
:name: fig:wt_sensor6
:width: 25em 

Sensor on breadboard
```


## 2 - Fixing the sensor to the top plate

Slide the pin header through the large hole and fix the sensor in place with the screws.

```{figure} ../_images/watchtower/light-sensor/sensor_7.jpg
:figwidth: 25em
:name: fig:wt_sensor7
:align: center

Final assembled sensor
```

## 3 - Wire the sensor to the Raspberry Pi

The best way to figure out how to do this is to always take the sensor and turn it until you can read what is written on it and that will be our basic position.

This is the pin layout, every number corresponding to a wire color (it doesn’t matter which color is which number, just make sure that the wire is connected to the right pin header).

Connections on the Raspberry Pi:

```
o o 1 o o

2 3 4 o 5
```

Connections on TCS34725:

```
o 1 2 4 3 o 5
```

```{figure} ../_images/watchtower/light-sensor/sensor_8.jpg
:figwidth: 25em
:name: fig:wt_sensor8
:align: center

Pin header Adafruit TCS34725
```

```{figure} ../_images/watchtower/light-sensor/sensor_9.jpg
:figwidth: 25em
:name: fig:wt_sensor9
:align: center

Pin header Raspberry Pi
```

You successfully wired the light sensor.
