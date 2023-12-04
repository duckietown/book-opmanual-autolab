(autolab-2-Duckietown-robot)=
# Duckietown robot

```{needget}
* An Raspberry Pi 4
---
* A Duckietown for the Duckietown in the Autolab
```

## What is a Duckietown robot?

Logically, a Duckietown robot oversees the communications in a Duckietown. So there is one Duckietown robot per Duckietown. Since the Duckietown robot only does computations and does not have actuators or sensors, it is simply a Raspberry Pi 4 connected to the same network as the other robots in the Duckietown.

## Initialize

Now you need to initialize the operating system of the Duckietown robot.
### Flash the SD card

The parameters:

- `concat_map_name` should be the name of the map for this Duckietown merged in the `duckietown/duckietown-world` Github repository, but concatenated over the underscore(`_`)s. E.g. For map name `ETH_large_loop`, the `concat_map_name` is `ETHlargeloop`.
- `country_code` is the [two-letter country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)

Command to flash:

```bash
dts init_sd_card --hostname ![concat_map_name] --type duckietown --configuration DT21 --country ![country_code]
```

### Connect to network and boot

The network connection should be wired. Insert the SD card into the Raspberry Pi 4 and power it on. You can monitor when the boot process is complete by checking if the Duckietown shows up in:

```bash
dts fleet discover
```

## Setup

### Navigate to the Duckietown dashboard and login

```{figure} ./_images/autobots_dashboard-login.png
:width: 80%
```

### Select "Settings" pane

```{figure} ./_images/autobots_dashboard-settings.png
:width: 80%
```

### Fill in the configs for "Autolab"

The `Map name` field requires a string, e.g. `ETH_large_loop`. It's the name of the map of the Duckietown created and merged to the `duckietown/duckeitown-world` GitHub repository.

```{figure} ./_images/autobots_dashboard-autolab-config.png
:width: 80%
```

```{attention}
Be careful not to confuse between `Map name` and `concat_map_name` when initializing the SD card.

```

```{note}
The `Tag id` field should be omitted for a Duckietown robot. (It does not have an Apriltag)

```

Click `Save and Apply` and refresh the page. The user inputs should appear in the fields now.
