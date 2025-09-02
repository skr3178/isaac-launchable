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

1. Click this Deploy Now button
[![ Click here to deploy.](https://brev-assets.s3.us-west-1.amazonaws.com/nv-lb-dark.svg)](https://brev.nvidia.com/launchable/deploy?launchableID=env-31ezDWyp4LvtDQr5rUhAWOUMFhn)
2. Click the Deploy Launchable button to spin up the instance.
3. Wait for the instance to be fully ready on Brev: running, built, and the setup script has completed (first launch can take a while)
4. On the Brev instance page, scroll to the TCP/UDP ports section.
5. Click the link for port 80 (HTTP) to open Visual Studio Code Server.
6. The default password is `password`. This can be modified.
7. Inside Visual Studio Code, continue with the [README.md](https://github.com/isaac-sim/isaac-launchable/blob/main/isaac-lab/vscode/README.md) instructions. A summary is provided below.

Once the environment is up, use Isaac Lab as normal. If the Isaac Sim UI is needed (e.g., for policy evaluation), enable streaming as described below.

To run an Isaac Lab sample:
`./isaaclab/isaaclab.sh -p "isaaclab/scripts/tutorials/00_sim/create_empty.py" --kit_args="--no-window --enable omni.kit.livestream.webrtc"`

### View the Isaac Sim UI in the browser
To run an Isaac sample:

1. Add these additional arguments to the Isaac Lab command `--kit_args="--no-window --enable omni.kit.livestream.webrtc"`.
2. Open a new tab in your browser.
3. Visit the same address as the Visual Studio Code server and append: `/viewer`

#### Example: 

VSCode - `ec2.something.something.amazonaws.com` at port 80

Isaac Sim UI - `ec2.something.something.amazonaws.com/viewer`

### Running Locally or Creating Your Own Launchable

The following instructions cover both local usage and creating a custom Brev Launchable based on this project.

#### Configuring a Custom Brev Launchable

These instructions are how you create a Launchable as linked at the beginning of this README. You can also fork this repo to customize the containers for your own projects and use cases.

1. Log in to the [Brev](https://login.brev.nvidia.com/signin) website.
2. Go to the Launchables category.
3. Click the **Create Launchable** button.
4. Choose the "I don't have any code files" option.
5. Choose "VM Mode - Basic VM with Python installed", then click Next.
6. On the next page, add a setup script. Under the *Paste Script* tab, add this code:
```bash
#!/bin/bash
git clone https://github.com/isaac-sim/isaac-launchable
cd isaac-launchable/isaac-lab
docker compose up -d
```
7. Click Next.
8. Under "Do you want a Jupyter Notebook experience" select "No, I don't want Jupyter".
9. Select the TCP/UDP ports tab.
10. Expose the following ports (for Visual Studio Code Server and Kit App Streaming):
```
80
1024
47998
49100
```
11. Click Next.
12. Choose your desired compute. 
Note: GPUs with RT cores are required for Kit App Streaming. The specs and drivers versions provided will also need to be compatible with [Isaac Sim](https://docs.isaacsim.omniverse.nvidia.com/5.0.0/installation/requirements.html). Unfortunately the available drivers are not exposed on this page currently.
13. Choose disk storage, then click Next.
14. Enter a name, then select **Create Launchable**

Congratulations! You now have a custom launchable.

#### Launching locally

This project can also be used to run a containerized version of Isaac Sim and Isaac Lab.

To use this project locally, you'll need a workstation that meets Isaac Sim's requirements.

#### Starting the stack

Simply run `docker compose up -d` using this file: `isaac-lab/docker-compose.yml`.

## Licensing Terms

By clicking the "Deploy Launchable" button, you agree to the NVIDIA Isaac Sim Additional Software and Materials License Agreement found here https://www.nvidia.com/en-us/agreements/enterprise-software/isaac-sim-additional-software-and-materials-license/.
