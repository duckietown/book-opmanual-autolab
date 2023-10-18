(autolab-2-map)=
# Map

```{needget}
- The city layout is fixed and ready.
---
- The city layout transcribed in `duckietown-world`, to be used for visualization tools, for localization and much more.
```

In this section, the user will formalize the Duckietown built to a predefined map format, and integrate the map into the `duckietown/duckietown-world` Github repository. The formalized map is required for subsequent operations, e.g. localization.

```{note}
The term `venv` used in texts of this document refers to the Python 3 virtual environments.
```

## Setting up duckietown-world

Before creating our map we need to set up the software we are going to use.

### Fork and clone the repository

Fork the repository [duckietown/duckietown-world](https://github.com/duckietown/duckietown-world) ([how-to](https://docs.github.com/en/get-started/quickstart/fork-a-repo)) and clone your fork to the local machine ([how-to](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository-from-github/cloning-a-repository)).

In the following instructions, replace the `[path/to]` with the path to where the repository is cloned to, and move to it by executing:

```bash
cd [path/to]duckietown-world
```

Install `git-lfs` and pull all lfs files:

```bash
sudo apt update -y && sudo apt install -y git-lfs
git-lfs
```

### Create the venv 

```{note}
`duckietown-world` currently needs Python 3.7 or higher. You should verify this before proceeding.
```

Install and create venv:

```bash
sudo apt update -y and sudo apt install -y python3-venv
python3 -m venv duckietown-world-venv
source duckietown-world-venv/bin/activate
```

When you need to get out of the venv, just run `deactivate`)

```{warning}
Don't add the venv files to git commits.
```

### Further setup in the venv

```bash
python3 -m pip install --upgrade pip
python3 -m pip install -r requirements.txt
python3 setup.py develop --no-deps
```

Note: in case the last command returns the error 'Permission denied: 'src/duckietown_world_daffy.egg-info/PKG-INFO ', go to the folder ```src/duckietown_world_daffy.egg-info``` and change permission for the folder itself as well as all the files within the folder with ```sudo chmod 777 *```. After doing this, run the last command again

## Create your map

To create a map interactively with the jupyter notebook, with your venv activated, execute inside the `duckietown-world`repository:

```bash
jupyter notebook
```

```{warning}
Please open the notebooks with browsers other than Firefox, which has a known bug with visuals of maps. Chrome works fine.
```

Now, navigate to the `notebooks` directory and open `30-DuckietownWorld-maps.ipynb`. Go through the steps in order to get familiar with the code functions until the `Creating new maps` section is reached. Once there, you can start creating your own new map. In the section below there you will find the tile names and representation to help you.

You can add your map line by line, until satisfied with the result. Once this is done:

* Adjust the tile size parameter. The size is the inner to outer size (on most tiles it is 0.585 meters).
* Create, in the `duckietown-world/src/duckietown_world/data/gd1/maps` directory, a new yaml file for your map (e.g. `ETH_small_loop.yaml`).
* Copy the working map contents that you just created in the juypter notebook to the yaml map file.
* Check that you have the tiles and the tile-size.

## Create map visualization files

Once `[MAP_NAME].yaml` file is composed, in the root of duckietown-world, with venv activated, run (without `.yaml`):

```bash
dt-world-draw-maps ![MAP_NAME]
```

This should generate a new folder with your map name in the `out-draw_maps` directory. You can now move this folder to the `visualization/maps/` folder.

```bash
mv out-draw_maps/![MAP_NAME] visualization/maps/
```

## Verification

If you now go back to the notebook and run the line that lists all maps, you should find yours in the list.

## Committing

```{warning}
Again, please do not commit any venv files.
```

Now that the map is ready, you can commit locally the following files:

* the map yaml file in `duckietown-world/src/duckietown_world/data/gd1/maps` 
* the generated visualization in `visualization/maps/`

Then push the local commit to your forked repository, and create a pull request to the official `duckietown/duckietown-world` repository ([how-to](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)).

In case the pull request is not reviewed in time, please ask for help in the `#devel-autolab` channel on the Duckietown Slack.
