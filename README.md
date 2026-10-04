# DriveForge — Deep Reinforcement Learning for Autonomous Driving

This project explores autonomous driving in the CARLA simulator using deep reinforcement learning. It includes a continuous-action Proximal Policy Optimization (PPO) agent, an experimental discrete-action DDQN agent, and a variational autoencoder (VAE) for compressing semantic camera observations.

> **Simulation only:** this software is intended for research and simulator use. It is not designed or validated for real vehicles or public-road driving.

## Project components

- **CARLA environment** — simulator connection, vehicle controls, sensors, and route/environment setup in `simulation/`.
- **PPO agent** — continuous-action policy and training/evaluation entry point in `continuous_driver.py` and `networks/on_policy/`.
- **DDQN agent** — experimental discrete-action implementation started from `discrete_driver.py` and `networks/off_policy/`.
- **VAE** — image encoder/decoder components in `autoencoder/`; `encoder_init.py` initializes the trained encoder for agent observations.

## Requirements and compatibility

- Windows or Linux
- Python 3.7 (64-bit) is the version specified by the original project dependencies
- CARLA simulator and Additional Maps for the towns you plan to use
- PyTorch compatible with your installed Python and CUDA version; CPU execution may be possible but can be substantially slower

**Match CARLA versions carefully.** The README and code target CARLA 0.9.14, while the bundled API egg is named for CARLA 0.9.8 and Python 3.7. The CARLA server and Python API client must use compatible versions. Replace or configure the bundled egg if it does not match your simulator installation.

## Installation

1. Install the CARLA server and the Additional Maps package from the [CARLA releases](https://github.com/carla-simulator/carla/releases). Install the map assets into the CARLA server installation as directed by CARLA.
2. Create a Python 3.7 64-bit environment compatible with the selected CARLA release.
3. Install the project dependencies from the repository root:

   ```bash
   pip install -r requirements.txt
   ```

4. Install a matching PyTorch build for your Python and CUDA setup. Use the official [PyTorch installation selector](https://pytorch.org/get-started/locally/) rather than assuming a particular CUDA version.
5. Make the matching CARLA Python API package available to the environment. The repository contains an egg under `carla/`; verify its version before using it.

## Run the simulator

Start the CARLA server before running a project script. Example server commands are shown below; use the executable path for your own installation.

Linux:

```bash
./CarlaUE4.sh -RenderOffScreen
```

Windows:

```powershell
.\CarlaUE4.exe -RenderOffScreen
```

Use the CARLA command-line options appropriate for your machine. The server must be running and reachable before launching a driver.

## Run a trained PPO agent

From the repository root, start the default trained agent (Town07):

```bash
python continuous_driver.py --exp-name ppo --train False
```

To select Town02:

```bash
python continuous_driver.py --exp-name ppo --train False --town Town02
```

Pretrained checkpoints are stored under `preTrained_models/PPO/`. Confirm the selected town has the expected checkpoint before starting evaluation.

## Train a PPO agent

```bash
python continuous_driver.py --exp-name ppo
```

Town07 is the default; pass `--town Town02` to select Town02. Training checkpoints are written under `checkpoints/PPO/<town>/`, and metrics are written under `logs/PPO/<town>/`. Review `parameters.py` for experiment defaults and available settings.

The discrete DDQN driver is experimental and marked as work in progress:

```bash
python discrete_driver.py
```

Check the script's argument parser and configuration before running it.

## VAE tools

The repository includes VAE model code and reconstruction utilities. To inspect reconstructed images using an available trained model and dataset:

```bash
cd autoencoder
python reconstructor.py
```

The original project describes training data collected from CARLA semantic-segmentation images. Dataset files and pretrained weights may need to be supplied locally.

## Monitoring

View TensorBoard summaries from the repository root:

```bash
tensorboard --logdir runs/
```

## Project structure

| Path | Description |
|---|---|
| `continuous_driver.py` | PPO training and evaluation entry point |
| `discrete_driver.py` | Experimental DDQN entry point |
| `parameters.py` | Project hyperparameters and settings |
| `simulation/` | CARLA connection, environment, sensors, and settings |
| `networks/on_policy/` | PPO agent and network implementation |
| `networks/off_policy/` | Off-policy networks and DDQN implementation |
| `autoencoder/` | VAE encoder, decoder, and reconstruction tools |
| `preTrained_models/` | Distributed pretrained agents |
| `checkpoints/` | Training checkpoints |
| `logs/`, `runs/` | Training metrics and TensorBoard data |
| `info/` | Documentation, figures, diagrams, and videos |

## References and credits

The original project credits **Umesh** as an author. The accompanying research document is available at [`info/documentation/`](info/documentation/).


This project uses [CARLA](https://carla.org/), [PyTorch](https://pytorch.org/), and [TensorBoard](https://www.tensorflow.org/tensorboard). See [LICENSE.md](LICENSE.md) for the project license and third-party terms.# DriveForge
