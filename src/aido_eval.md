# Evaluator

This diagram presents the abstract hierarchy among different layers involved in the Autolab evaluation for AIDO.

![dt_autolab_aido_block_diagram.jpeg](./_images/localization-operations/dt_autolab_aido_block_diagram.jpeg)

- Autolab Locations:
  - ETHZ
  - TTIC

## Entity Definition

| Entity                                                                                        | Description                                                                                          | Num Required | Configuration   |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| Lab Operator                                                                                  | Humans at the autolab, for running programs, placing duckies etc.                                  | 1            | N/A             |
| Evaluation Runner                                                                             | A local Ubuntu machine that coordinates the evaluation process...                                  | 1            | Ubuntu 20.04    |
| AIDO Server                                                                                   | The server that distributes submissions to be evaluated...                                           | 1            |                 |


## AIDO Evaluator

To run AIDO submissions, you need to run the localization pipeline in REST mode. This will create a REST API that the AIDO evaluator process will use to run localization experiments on the Autolab. It is basically a server for accessing the pose-graph optimizer.

You can do so by running the following command from the root of the `dt-autolab-localization` repository:

```bash
dts devel build
dts devel run -f -X -L REST -- --hostname ETHlargeloop
```

The AIDO evaluator, which is the process that talks to the challenge server and gets assigned jobs, can be found in [this repository](https://github.com/duckietown/dt-aido-autolab-evaluator). The evaluator needs a working directory to store temporary files in. The command below assumes that the directory `/data` exists on your computer. Make sure you create it first, or you might get errors about some files being missing. So:

```bash
sudo mkdir /data
```

You can now run it by executing the following command from the root of its repository:

```bash
dts devel run \
  -A aido=5 \
  -A autolab=ETH_large_loop \
  -A token=<YOUR_DT_TOKEN> \
  -A stage \
  -X \
  -- -v /var/run/docker.sock:/var/run/docker.sock \
  -e DEBUG=1
```
