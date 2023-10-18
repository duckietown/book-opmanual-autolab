(light-sensor-demo)=
# Light sensors demo

```{todo}
Add the name of the right watchtower spec
```

```{needget}
- A fully operational Duckietown with [watchtowers](watchtower-hardware).

- A sensor which is correctly setup on a running watchtower.

- An external light sensor that measures luminescence in `lux` to be used as reference.

- An environment (even very small) where you can control the light intensity (try to use two light intensities: one below and one above the operating point).

---
- Accurate measure of the light field in the Autolab.
```

## Introduction

To calibrate the sensor we need to measure the light at two different intensities in order to be able to make a linear approximation. To do so we measure two different light intensities, input the expected values to the sensor and then make a linear approximation.

We can measure the two intensities by running the container and following the instructions on the console (don’t forget that you need an external reference sensor as well).

```{todo}
Complete instructions for calibration of the light sensor
```

### Running the calibration
You can pull the image of the docker container to the agent

```{todo}
Check calibration of sensor

You can check the calibration by going to the calibration folder, TODO
```

## Running the demo

To run the sensor demo you first need to pull the image:

```bash
docker -H [!HOSTNAME].local pull gian1717/sensor:p2
```

Then you can start the container:

```bash
docker -H [!HOSTNAME].local run -it  --net host --privileged --name light-sensor -v /data:/data gian1717/sensor:p2
```
