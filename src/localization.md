=(autolab-2-localization)
# Operation: Localization

- Autolab Locations:
  - ETHZ
  - TTIC
- Maps Used:
  - ETH_large_loop/TTIC_large_loop
- [Map SVG visualization](https://github.com/duckietown/duckietown-world/blob/daffy/visualization/maps/ETH_large_loop/drawing.svg)

## Architecture overview

```{note}
More description about each entity is provided in the next section.
```

This diagram presents the abstract hierarchy among different layers involved in the Autolab evaluation for AIDO.

![dt_autolab_aido_block_diagram.jpeg](./_images/dt_autolab_aido_block_diagram.jpeg)

## Entity Definition

| Entity                                                                                        | Description                                                                                          | Num Required | Configuration   |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| Lab Operator                                                                                  | Humans at the autolab, for running programs, placing duckies etc.                                  | 1            | N/A             |
| Duckies                                                                                       | Rubber ducks, as pedestrians in the city/autolab                                                     | 10           | Color: Yellow   |
| [Autobots](https://docs.duckietown.org/daffy/opmanual_autolab/out/autolab_autobot_specs.html) | The Duckiebots with top plate and Apriltags. Where the AIDO submissions will be installed and run. | 3            | DB22            |
| [Watchtowers](https://docs.duckietown.org/daffy/opmanual_autolab/out/watchtower_hardware.html) | Cameras in the autolab that continue to recognize Apriltags (on the ground and Duckiebots).       | 6/7          | WT19B           |
| Duckietown                                                                                    | The robot that collects the transforms published from all other robots.                             | 1            | DT20            |
| Evaluation Runner                                                                             | A local Ubuntu machine that coordinates the evaluation process...                                  | 1            | Ubuntu 20.04    |
| AIDO Server                                                                                   | The server that distributes submissions to be evaluated...                                           | 1            |                 |


## Operation instructions

1. Install `docker-compose` for the desktop machine: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)
2. Make sure you are on the `daffy` version of the shell commands with:

   ```
   dts --set-version daffy
   ```

3. Update the commands:

   ```
   dts update
   ```

4. On the local machine, for **ALL** robots (Autobot, Watchtower, Duckietown):

   ```
   dts duckiebot update [HOSTNAME]
   ```

5. On robots, based on the robot type, create some configs:
    
    - For **ALL** robots (Autobot, Watchtower, Duckietown):
        Replace `[MAP_NAME]` with the map name for this Autolab, e.g. `ETH_large_loop` or `TTIC_large_loop`:

        ```
        echo [MAP_NAME] > /data/config/autolab/map_name
        ```

    ```{attention}
    Autobot only!
    ```

    - Replace `[TAG_ID]` with the AprilTag ID (just the number) on top of that Autobot:

        ```
        echo [TAG_ID] > /data/config/autolab/tag_id
        ```

6. On **Watchtowers** only, make sure that the `dt-core` container is running. You can do so by running this on your desktop/laptop:

    ```
    dts stack up -H [HOSTNAME] core -d
    ```

    ```{tip}
    Make sure the AprilTag detector is spinning and you get detection messages in `rostopic hz`.
    ```

7. On the local machine, for **ALL** robots (Autobot, Watchtower, Duckietown), we need the Autolab layer running as well. You can do so by running this on your desktop/laptop:

    ```
    dts stack up -H [HOSTNAME] autolab -d
    ```

    ```{note}
    This guide was written for the map `ETH_large_loop`. Replace it with your map name every time you find it in a command.
    ```

9. Check if you get data. Clone the repo [https://github.com/duckietown/dt-autolab-localization](https://github.com/duckietown/dt-autolab-localization) and run the following command from its root:

    ```
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

   Make sure you get detections from all the watchtowers. Every 10s, the autobots will publish their TF between their footprint and the AprilTag on top of them:

   ```
   watchtower04/camera_opt



Another way of looking at this data is by running the following command (from the root of the same repository):

```bash
dts devel run -f -X -L single-experiment -- --hostname ETHlargeloop
```

This should wait for 12 seconds for all these observations to come in and put everything in a pose-graph that will be optimized and rendered. You should see something like this appear:

![image-20201207-232938.png](./_images/image-20201207-232938.png)

## Integrate Odometry data (wheel encoders)

In order to integrate encoder data into the localization system, you need an extra container that takes the odometry data from the robot and feeds it into the localization system.

::: note NOTE
You need to run this on every **Autobot**.
:::

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

## AIDO Evaluator

To run AIDO submissions, you need to run the localization pipeline in REST mode. This will create a REST API that the AIDO evaluator process will use to run localization experiments on the Autolab. It is basically a server for accessing the pose-graph optimizer.

You can do so by running the following command from the root of the `dt-autolab-localization` repository:

```bash
dts devel build
dts devel run -f -X -L REST -- --hostname ETHlargeloop
```

The AIDO evaluator, which is the process that talks to the challenge server and gets assigned jobs, can be found in this repository [https://github.com/duckietown/dt-aido-autolab-evaluator](https://github.com/duckietown/dt-aido-autolab-evaluator). The evaluator needs a working directory to store temporary files in. The command below assumes that the directory `/data` exists on your computer. Make sure you create it first, or you might get errors about some files being missing. So:

```bash
sudo mkdir /data
```

You can now run it by executing the following command from the root of its repository:

```bash
dts devel run \
  -A aido=5 \
  -A autolab=ETH_large_loop \
  -A token=<YOUR_TOKEN> \
  -A stage \
  -X \
  -- -v /var/run/docker.sock:/var/run/docker.sock \
  -e DEBUG=1
```
