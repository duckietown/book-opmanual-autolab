(autolab-2-localization)=
# Operation: Localization

- Default available maps:
  - `ETH_large_loop`/`TTIC_large_loop`

```{figure} ./_images/localization-operations/eth_large_loop.png
:width: 40em

ETH_large_loop
```

## Entity Definition

| Entity                                                                                        | Description                                                                                          | Num Required | Configuration   |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| Duckies                                                                                       | Rubber ducks, as pedestrians in the city/autolab                                                     | 10           | Color: Yellow   |
| [Autobots](https://docs.duckietown.org/daffy/opmanual_autolab/out/autolab_autobot_specs.html) | The Duckiebots with top plate and Apriltags. Where the AIDO submissions will be installed and run. | 3            | DB22            |
| [Watchtowers](https://docs.duckietown.org/daffy/opmanual_autolab/out/watchtower_hardware.html) | Cameras in the autolab that continue to recognize Apriltags (on the ground and Duckiebots).       | 6/7          | WT19B           |
| Duckietown                                                                                    | The robot that collects the transforms published from all other robots.                             | 1            | DT20            |

## Operation instructions

In order to operate your autolab you need to set it up according to the following instructions:

1. Install `docker-compose` for the desktop machine: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
2. Make sure you are on the `ente` version of the shell commands by running:

    ```bash
    dts --set-version ente
    ```

3. Update the commands:

    ```bash
    dts update
    ```

4. On the local machine, for **ALL** robots (Autobot, Watchtower, Duckietown):

    ```bash
    dts duckiebot update [HOSTNAME]
    ```

5. On robots, based on the robot type, create the following configs:
    
    - For **ALL** robots (Autobot, Watchtower, Duckietown):
        Replace `[MAP_NAME]` with the map name for this Autolab, e.g. `ETH_large_loop` or `TTIC_large_loop`:

        ```bash
        echo [MAP_NAME] > /data/config/autolab/map_name
        ```

    ```{attention}
    Autobot only!
    ```

    - Replace `[TAG_ID]` with the AprilTag ID (just the number) on top of that Autobot:

        ```bash
        echo [TAG_ID] > /data/config/autolab/tag_id
        ```

6. On **Watchtowers** only, make sure that the `dt-core` container is running. You can do so by running this on your desktop/laptop:

    ```bash
    dts stack up -H [HOSTNAME] core -d
    ```

    ```{todo}
    add apriltag detector topic name
    ```

    ```{tip}
    Make sure the AprilTag detector is spinning and you get detection messages in `rostopic hz`.
    ```

7. On the local machine, for **ALL** robots (Autobot, Watchtower, Duckietown), we need the Autolab layer running as well. You can do so by running this on your desktop/laptop:

    ```bash
    dts stack up -H [HOSTNAME] autolab -d
    ```

    ```{note}
    This guide was written for the map `ETH_large_loop`. Replace it with your map name every time you find it in a command.
    ```

9. Check if you get data by cloning the repo [https://github.com/duckietown/dt-autolab-localization](https://github.com/duckietown/dt-autolab-localization) and running the following command from its root:

      ```bash
      dts devel build -f
      dts devel run -L test-tf -- --hostname ETHlargeloop
      ```

      ```{attention}
      The separator `--` is not a typo; the hostname is the map name without underscores, so `ETHlargeloop` instead of `ETH_large_loop`. The map name is **case sensitive**.
      ```

      You should see an output like this:

    ```
    watchtower03/camera_optical_frame tag/326
    watchtower03/camera_optical_frame tag/307
    watchtower03/camera_optical_frame tag/326
    watchtower02/camera_optical_frame tag/308
    watchtower02/camera_optical_frame tag/322
    ```

    Make sure you get detections from all the watchtowers. Every 10s, the autobots will publish the transform between their footprint and the AprilTag on top of them:

    ```
    watchtower04/camera_opt

Another way of looking at this data is by running the following command (from the root of the same repository):

```bash
dts devel run -f -X -L single-experiment -- --hostname ETHlargeloop
```

This should wait 12 seconds for the observations to come in and put everything in a pose-graph that will be optimized and rendered. You should see something like this appear:

![image-20201207-232938.png](./_images/localization-operations/image-20201207-232938.png)

## Integrate Odometry data (wheel encoders)

In order to integrate encoder data into the localization system, you need an extra container that takes the odometry data from the robot and feeds it into the localization system.

```{note}
You need to run this on every **Autobot**.
```

You can run the odometry integration with the command:

```bash
dts stack up -H [HOSTNAME] encoder -d
```

You can test that the data is correctly received by the localization system by running the `test-tf` application as done above. You should see something like the following, indicating that a transformation between the reference frame `footprint` of the robot at different timestamps is received:

```plaintext
autobot01/footprint autobot01/footprint
autobot01/footprint autobot01/footprint
autobot01/footprint autobot01/footprint
```
