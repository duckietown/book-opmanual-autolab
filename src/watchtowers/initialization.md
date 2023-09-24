(watchtower-initialization)=
# Initialization

```{needget}

* An SD card with a minimum size of 32 GB.

* A computer running **Ubuntu OS** (for flashing the SD card), an internet connection, an SD card reader, and at least 16 GB of free space.

* Duckietown Shell, Docker, and other configurations as specified in [Duckiebot Laptop Setup](book-opmanual-duckiebot:laptop-setup).

* A Duckietown Token set up as described in [Duckiebot Account Configuration](book-opmanual-duckiebot:dt-account).

---
* A ready Watchtower.
```

## Flashing the SD card

The image setup procedure for Watchtowers is the same as for [Duckiebots](book-opmanual-duckiebot:burn-the-sd-card-cli).

In the Autolab of ETH Zurich, we use the following naming convention:

- Linux username: `mom`
- Hostname: `watchtowerXX` (where `XX` specifies the number of the Watchtower)
- Password: `MomWatches`.

````{attention}

Please add `--type watchtower` to the flashing procedure.


For Raspberry Pi 4, add `--experimental` to the command.
````

A complete command will look like:

```bash
laptop $ dts init_sd_card --hostname watchtower![XX] --linux-username mom --linux-password MomWatches --country ![COUNTRY] --type watchtower --experimental
```

Using the above naming conventions, you can flash your SD cards as described in [Duckiebot Initialization](book-opmanual-duckiebot:setup-duckiebot).



## Calibrating the camera

Using the instructions in [Camera Calibration](book-opmanual-duckiebot:camera-calib), you should perform only the intrinsic calibration for the Watchtowers.

```{note}
Be sure to check the quality of the image. It should be `1296x972` pixels, not `480x640` pixels like on the Autobots. If it is not, this means you didn't flash the Watchtower with the `--type watchtower` argument. To resolve this, reflash it.

```