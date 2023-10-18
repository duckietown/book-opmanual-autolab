(autolab-2-autobots)=
# Autobot

```{needget}
* A [Duckiebot](duckiebot-configurations) in `DB21` or `DB19` configuration
---
* An Autobot ready for the localization system and AIDO evaluations
```

In this section, the user will modify a standard Duckiebot for Autolab operations.

```{note}
The term `tag(s)` refers to AprilTag(s).
```

## Reflash SD cards for unified naming

If the Duckiebot is calibrated, back up the camera and kinematics calibration files.

```{attention}
Use these conventional Autobot hostnames to flash the SD card:

- autobot`XX` (where `[XX]` are fixed 2 digits starting from `01`)

```

Restore the calibration files (if applicable).

## Hardware modification

An AprilTag will be mounted on top of the Autobot for localization.

- Download the [AprilTags for Autobots](https://github.com/duckietown/docs-resources_autolab/blob/daffy/AprilTags/Apriltags_autobots.pdf) file, select the range of tags needed (corresponding to the hostnames setup when flashing SD cards) and print the relevant pages.
- Along the ruler lines on edges of the pages, cut out each tag.
- Apply configuration-wise changes and alignment, fix the tag in place.

### DB21

```{figure} ./_images/autobots_db21m.jpg
:name: fig:db21
:width: 80%

DB21 Autobot
```

As shown, align the top of the tag with the bottom of fan screws, trim the dangling white paper.

### DB19

Prepare the materials:

- 4 standoffs M3 x 50mm, for example from [here](https://www.distrelec.ch/en/spacer-bolt-6mm-50-mm-no-brand-distin3060pa-50/p/14843056?queryFromSuggest=true)
- 8 screws M3 * 8mm
- laser cut [top plate](https://github.com/duckietown/docs-resources_autolab/tree/daffy/Topplate_AprilTag)

```{figure} ./_images/autobots_db19-materials.jpg
:name: fig:db19-materials
:width: 80%

Material needed for modifying DB19
```

Place the tag in the slot on the top plate:

```{figure} ./_images/autobots_db19-at-placement.jpg
:name: fig:db19-at-placement

Placement of tag
```

Install the standoffs as shown:

```{figure} ./_images/autobots_db19.jpg
:width: 80%
:name: fig:db19-apriltag

DB19 Autobot
```
## Software setup

This setup is the same for all hardware configurations.

### Navigate to the Autobot dashboard, login

```{figure} ./_images/autobots_dashboard-login.png
:width: 80%
:name: fig:dashboard-login

Autobot Dashboard Login
```

### Select "Settings" pane

```{figure} ./_images/autobots_dashboard-settings.png
:width: 80%
:name: fig:dashboard-settings

Dashboard Settings
```

### Fill in the configs for "Autolab"

```{figure} ./_images/autobots_dashboard-autolab-config.png
:width: 80%
:name: fig:dashboard-autolab-config

Dashboard Autolab Config
```

The `Map name` field requires a string, e.g. `ETH_large_loop`. It's the name of the map of the Duckietown created and merged to the `duckietown/duckeitown-world` Github repository.

The `Tag id` field requires 3 digit integers, e.g. `403`. With the provided AprilTags, the `Tag id`s correspond to Autobot hostnames in the following way:

- autobot01 - Tag id 400
- autobot02 - Tag id 401
- autobot03 - Tag id 402
- ...

Click `Save and Apply` and refresh the page. The user inputs should appear in the fields now.

````{admonition} Where do the configs go?
:class: info

These steps update the configurations stored in `/data/config/autolab/tag_id` and `/data/config/autolab/map_name` files on the Autobot, which are later used. 

They are equivalent to connecting through ssh and changing the files directly on the Autobot.
````
