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
| [Autobots](https://docs.duckietown.org/daffy/opmanual_autolab/out/autolab_autobot_specs.html) | The Duckiebots with top plate and Apriltags. Where the AIDO submissions will be installed and run. | 3            | DB19            |
| [Watchtowers](https://docs.duckietown.org/daffy/opmanual_autolab/out/watchtower_hardware.html) | Cameras in the autolab that continue to recognize Apriltags (on the ground and Duckiebots).       | 6/7          | WT19B           |
| Duckietown                                                                                    | The robot that collects the transforms published from all other robots.                             | 1            | DT20            |
| Evaluation Runner                                                                             | A local Ubuntu machine that coordinates the evaluation process...                                  | 1            | Ubuntu 20.04    |
| AIDO Server                                                                                   | The server that distributes submissions to be evaluated...                                           | 1            |                 |

---

## Ideal Evaluation Process (just for understanding)

- [x] [@Andrea F Daniele](https://ethidsc.atlassian.net/wiki/people/557058:3a064d75-0053-474f-909a-dbcb43cf5c72) fill in what’s missing/wrong here

1. [Evaluation Runner] requests a submission from the [AIDO Server]
2. [Evaluation Runner] extracts the challenge definition, e.g. where to place [Autobots] and [Duckies] and shows that to the operator.
   Interface:
   ![image-20201207-095809.png](./attachments/image-20201207-095809.png)
3. [Lab Operator] places the objects as instructed
4. [Evaluation Runner] copies the config directory from the robots to the [Evaluation Runner] disk
5. [Evaluation Runner] runs the participant’s code locally which will drive the [Autobots]
6. [Evaluation Runner] starts the bag recording on each device
7. [Evaluation Runner] starts a localization experiment which will output a trajectory for each [Autobot]
8. [Lab Operator] can stop the evaluation at any time and abort the evaluation.
9. [Evaluation Runner] downloads the ROS bags from the robots to the [Evaluation Runner] disk
10. [Evaluator Runner] uploads files (challenge scenario/rosbags/robot configs/trajectories etc.) and reports to the [AIDO Server]
11. [AIDO Server] uses the evaluation result to calculate the scores and compile videos for presenting on the AIDO web page

---

## Operation manual

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
   - **Autobot ONLY**:
     - Replace `[TAG_ID]` with the AprilTag ID (just the number) on top of that Autobot:
       ```
       echo [TAG_ID] > /data/config/autolab/tag_id
       ```
   - For **ALL** robots (Autobot, Watchtower, Duckietown):
     - Replace `[MAP_NAME]` with the map name for this Autolab, e.g. `ETH_large_loop` or `TTIC_large_loop`:
       ```
       echo [MAP_NAME] > /data/config/autolab/map_name
       ```
6. On **Watchtowers** ONLY, make sure that the `dt-core` container is running. You can do so by running this on your desktop/laptop:
   ```
   dts stack up -H [HOSTNAME] core -d
   ```
   Make sure the AprilTag detector is spinning and you get detection messages in `rostopic hz`.
7. On the local machine, for **ALL** robots (Autobot, Watchtower, Duckietown), we need the Autolab layer running as well. You can do so by running this on your desktop/laptop:
   ```
   dts stack up -H [HOSTNAME] autolab -d
   ```
8. **NOTE:** This guide was written for the map `ETH_large_loop`. Replace it with your map name every time you find it in a command.
9. Check if you get data. Clone the repo [https://github.com/duckietown/dt-autolab-localization](https://github.com/duckietown/dt-autolab-localization) and run the following command from its root:
   ```
   dts devel build -f
   dts devel run -L test-tf -- --hostname ETHlargeloop
   ```
   **NOTE:** The separator `--` is not a typo; the hostname is the map name without underscores, so `ETHlargeloop` instead of `ETH_large_loop`. You should see lines like these:
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