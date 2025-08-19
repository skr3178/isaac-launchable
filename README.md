# Isaac Launchable

Isaac Launchable offers a simplified approach to installing and using [Isaac Lab](https://isaac-sim.github.io/IsaacLab/main/index.html), the NVIDIA framework for robot learning.

The "Launchable" name refers to [NVIDIA Brev](https://developer.nvidia.com/brev) Launchables, which are the intended use of this repository. Launchables are preconfigured, fully optimized compute and software environments. They allow users to start projects without extensive setup or configuration.

Isaac Lab is built upon Isaac Sim, the NVIDIA simulation application for robotics applications. Isaac Sim can both be launched separately, or from the context of Isaac Lab using the `--sim` flag.

### What this project contains
The installation steps for Isaac Lab are automated via Docker, such that it can be used locally, or be deployed on services such as NVIDIA Brev and run with cloud resources.

The project includes:
- a Visual Studio Code container
- Isaac Lab pre-installed
- Isaac Sim pre-installed
- an Omniverse Kit App Streaming client, based on the [web-viewer-sample](https://github.com/NVIDIA-Omniverse/web-viewer-sample) project.

Through this project, users can interact with Isaac Sim and Isaac Lab purely from a web browser, with one tab running Visual Studio Code and terminal commands such as headless Isaac Sim instances, and another tab representing the Kit App Streaming and user interface for Isaac Sim.

### Required Ports
The following ports need to be opened, for both the Visual Studio container and for Kit App Streaming.
```
80
1024
47998
49100
```
If you're creating a custom Launchable, these ports can opened from the Brev interface.

## Isaac Sim

To run [isaac-sim@4.5.0](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/isaac-sim) headless and stream it to the [web-viewer-sample](https://github.com/NVIDIA-Omniverse/web-viewer-sample).

```
cd isaac-sim
docker compose up -d
open $(docker compose logs web-viewer | grep Network | awk '{print $5}')
```

## Isaac Lab

To run Isaac Lab:

```
cd isaac-lab
docker compose up -d
# Open up the VSCode instance in your browser
open localhost:80 

# From within VSCode, open a terminal and launch Isaac Sim
isaacsim isaacsim.exp.full.streaming.kit --no-window

# Open another browser window or tab, and visit
localhost/viewer

# Now run Isaac Lab
./isaaclab.sh -p "scripts/tutorials/00_sim/create_empty.py" --experience isaacsim.exp.full.streaming.kit --kit_args="--no-window"
```

To run other Isaac Lab commands, simply append these parameters to the command as shown above.
```
--experience isaacsim.exp.full.streaming.kit --kit_args="--no-window"
```

## Licensing Terms

By clicking the  "Deploy Launchable" button, you agree to the NVIDIA Isaac Sim Additional Software and Materials License Agreement found here https://www.nvidia.com/en-us/agreements/enterprise-software/isaac-sim-additional-software-and-materials-license/.
