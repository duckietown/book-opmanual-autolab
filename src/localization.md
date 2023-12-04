(autolab-2-localization)=
# Operation: Localization

- Default available maps:
  - `ETH_large_loop`/`TTIC_large_loop`

```{figure} ./_images/localization-operations/eth_large_loop.png
:width: 40em

ETH_large_loop
```

## Operation instructions

In order to operate your autolab you need to set it up according to the following instructions:

1. Install `docker-compose` for the desktop machine: [instructions](https://docs.docker.com/compose/install/)
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

9. Check if you get data by cloning [the autolab localization repo](https://github.com/duckietown/dt-autolab-localization) and running the following command from its root:

    ```bash
    dts devel build -f
    dts devel run -L test-tf -- --hostname ETHlargeloop
    ```

    ```{attention}
    The separator `--` is not a typo; the hostname is the map name without underscores, so `ETHlargeloop` instead of `ETH_large_loop`. The map name is **case sensitive**.
      ```

    You should see an output like this:

    ```bash
    watchtower03/camera_optical_frame tag/326
    watchtower03/camera_optical_frame tag/307
    watchtower03/camera_optical_frame tag/326
    watchtower02/camera_optical_frame tag/308
    watchtower02/camera_optical_frame tag/322
    ```

    Make sure you get detections from all the watchtowers. Every 10s, the autobots will publish the transform between their footprint and the AprilTag on top of them:

    ```bash
    watchtower04/camera_optical_frame tag/314
    autobot01/footprint tag/403
    watchtower05/camera_optical_frame tag/301
    ```

    The Duckietown robot will also publish TF between the map frame and each ground tag (every 10 seconds):

    ```bash
    watchtower02/camera_optical_frame tag/343
    world map
    map tag/301
    map tag/303
    map tag/309
    map tag/310
    map tag/311
    map tag/312
    map tag/314
    map tag/315
    map tag/318
    map tag/317
    map tag/321
    map tag/322
    map tag/307
    map tag/326
    map tag/343
    watchtower05/camera_optical_frame tag/301
    watchtower05/camera_optical_frame tag/307
    ```

    Make sure you get **all** these types of observations.
    
Another way of looking at this data is by running the following command (from the root of the same repository):

```bash
dts devel run -f -X -L single-experiment -- --hostname ETHlargeloop
```

This should wait 12 seconds for the observations to come in and put everything in a pose-graph that will be optimized and rendered. You should see something like this appear:

![image-20201207-232938.png](./_images/localization-operations/image-20201207-232938.png)

## Online localization

To execute localization in realtime (i.e. online) you can run the following command, again from the root of the repository:


```bash
dts devel run -f -X -L single-experiment-online -- --hostname ETHlargeloop
```

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

```
autobot01/footprint autobot01/footprint
autobot01/footprint autobot01/footprint
autobot01/footprint autobot01/footprint
```
