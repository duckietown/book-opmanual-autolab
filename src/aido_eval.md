(autolab-2-aido-eval)=
## AIDO evaluator

The AIDO evaluator is the process that talks to the AIDO challenges server and gets assigned evaluation jobs. After running an evaluation experiment, it uploads the artifacts (rosbags, logs, videos, localization results) for the AIDO challenge server to process further.

This diagram presents the abstract hierarchy among different layers involved in the Autolab evaluation for AIDO.

![dt_autolab_aido_block_diagram.jpeg](./_images/localization-operations/dt_autolab_aido_block_diagram.jpeg)

## Entity Definition

```{list-table} Entity Description
:header-rows: 1
:name: label-to-reference

* - Entity
  - Description
  - Num Required
  - Configuration
* - Lab Operator
  - Humans at the autolab, for running programs, placing duckies etc.
  - 1
  - N/A
* - Evaluation Runner
  - A local Ubuntu machine that coordinates the evaluation process...
  - 1
  - Ubuntu 20.04
* - AIDO Server
  - The server that distributes submissions to be evaluated...
  - 1
  -
```

# Operation: AIDO Evaluations

```{needget}
* [](autolab-2-localization) is set up.
* In addition to the desktop/laptop requirement for localization, an NVIDIA GPU is needed, and drivers should be installed.
---
* Can run AIDO submissions and report results.
``` 


### Run localization in REST mode

This creates REST APIs that the AIDO evaluator process will use to run localization experiments with the Autolab. From the root of the `duckietown/dt-autolab-localization` repository:

```bash
dts devel build
dts devel run -f -X -L REST -- --hostname ETHlargeloop
```

### Run the AIDO evaluator

Clone the Github repository `duckietown/dt-aido-autolab-evaluator`.

```bash
git clone https://github.com/duckietown/dt-aido-autolab-evaluator.git
```

The evaluator needs a directory, `/data`, to store temporary files in. Make sure to create it before running the evaluator.

```bash
mkdir -p /data
```

Then, from the root of the repository, run:

```bash
dts devel run \
    -A aido=5 \
    -A autolab=![name_of_the_map] \
    -A token=![your_duckietown_token] \
    -A stage \
    -X \
    -- -v /var/run/docker.sock:/var/run/docker.sock \
    -e DEBUG=1
```

## Troubleshooting
```{todo}
TODO: write common issues and resolutions
```
