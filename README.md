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

### Quickstart Guide
This guide will get you started with a Visual Studio Code instance with Isaac Lab preinstalled, and an in-browser user interface provided by Kit App Streaming.

1. Click the Deploy Now button below
[![ Click here to deploy.](https://brev-assets.s3.us-west-1.amazonaws.com/nv-lb-dark.svg)](https://brev.nvidia.com/launchable/deploy?launchableID=env-31ezDWyp4LvtDQr5rUhAWOUMFhn)

2. Wait for the the instance to be fully ready on Brev. The first time, this will take a while. Wait for it to be running, built, and for the setup script is complete.
3. Scroll down to the TCP/UDP ports section of the Brev instance page.
4. Click the link associated with port 80 - this will take you to Visual Studio Code Server.
5. The default password is `password`. This can be modified.
6. Continue with the [README.md](https://github.com/isaac-sim/isaac-launchable/blob/main/isaac-lab/vscode/README.md) instructions inside Visual Studio code. A summary is provided below.

Now you can run Isaac Lab as normal, unless you need to see the UI of Isaac Sim (for example, when evaluating a policy)
To do this:
1. Add these additional arguments to the Isaac Lab command `--kit_args="--no-window --enable omni.kit.livestream.webrtc"`.
2. Open a new tab in your browser.
3. Visit the same address as the Visual Studio Code server, but with a `/viewer` path appended.

Example: 

VSCode - `ec2.something.something.amazonaws.com` at port 80

Isaac Sim UI - `ec2.something.something.amazonaws.com/viewer`


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

To run [isaac-sim@5.0.0](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/isaac-sim) headless and stream it to the [web-viewer-sample](https://github.com/NVIDIA-Omniverse/web-viewer-sample).

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
