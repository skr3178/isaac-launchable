# IsaacLab Learning Content

## Running IsaacSim

You can run the streaming IsaacSim application at anytime with the following command:

```console
isaacsim isaacsim.exp.full.streaming.kit --no-window
```

Then, in a separate browser window, open `127.0.0.1:5173`.

## Running IsaacLab

You can run any of the IssacLab scripts with the streaming IsaacSim experience with the following command:

```console
./isaaclab.sh -p "scripts/tutorials/00_sim/create_empty.py" --kit_args="--no-window --enable omni.kit.livestream.webrtc"
```

Then, in a separate browser window, open `127.0.0.1:5173`.
