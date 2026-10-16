# OpenRoSDE Platform HSR

The Toyota HSR (Human Support Robot) platform of [OpenRoSDE](https://github.com/openrosde/openrosde), read "Open Robotic SDE", the open-source Software Development Environment for collaborative robotics research maintained by [Coarobo GK](https://coarobo.com/).
It is an extension that OpenRoSDE finds by name once it is cloned into `extensions/openrosde-platform-hsr/` of the core.

[![Test with OpenRoSDE](https://github.com/openrosde/openrosde-platform-hsr/actions/workflows/test-with-openrosde.yml/badge.svg?branch=main)](https://github.com/openrosde/openrosde-platform-hsr/actions/workflows/test-with-openrosde.yml)
[![ROS Noetic](https://img.shields.io/badge/ROS_Noetic-Framework-blue?logo=ros&logoColor=white)](http://wiki.ros.org/noetic)
[![Gazebo](https://img.shields.io/badge/Gazebo-Simulator-blue)](http://gazebosim.org/)
[![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-green)](LICENSE)

## 📑 Content

[[_TOC_]]

## 🤖 Robot Platform

| Platform | Robot                                                              | Documentation                                              |
| -------- | ------------------------------------------------------------------ | ---------------------------------------------------------- |
| `hsr`    | [Toyota HSR (Human Support Robot)](https://github.com/hsr-project) | [docs/robot-platforms/hsr.md](docs/robot-platforms/hsr.md) |

## 📁 Directory Structure

| Path                             | Content                                                                                                           |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `env/hsr.env`                    | Default setup of the platform, which `bash ./SWITCH-ROBOT-PLATFORM.bash hsr` copies to `.env.custom` of the core. |
| `docker/hsr-devel/Dockerfile`    | Platform image `hsr-devel`, built on the image `openrosde-base` of the core.                                      |
| `docker/docker-compose-hsr.yaml` | Docker Compose overlay of the platform.                                                                           |
| `ros/hsr_catkin_ws/`             | Catkin workspace with `openrosde_hsr_launch` and `openrosde_hsr_behaviors`.                                       |
| `tmux/hsr.panes`                 | Panes of the `tmux` sessions of the platform.                                                                     |
| `terminator/hsr.config`          | Terminator layouts of the platform, until `tmux` replaces them.                                                   |
| `scripts/hsr/`                   | Bringup of the robot.                                                                                             |
| `docs/robot-platforms/hsr.md`    | Documentation of the platform.                                                                                    |

The layout is the one of the core, which the [Extensions](https://github.com/openrosde/openrosde/blob/dev/docs/extensions.md) guide of OpenRoSDE describes.

## 🚀 Quick Start

1. Clone and set up the core of OpenRoSDE on the Ubuntu development machine, as its [Quick Start](https://github.com/openrosde/openrosde#-quick-start) describes:

    ```shell
    git clone https://github.com/openrosde/openrosde.git && cd openrosde/
    bash ./SETUP-DEVEL-MACHINE.bash
    ```

2. Clone this platform into `extensions/` of the core:

    ```shell
    git clone https://github.com/openrosde/openrosde-platform-hsr.git extensions/openrosde-platform-hsr
    ```

3. Switch to the platform, which also clones the Git submodules of every extension and applies their patches:

    ```shell
    bash ./SWITCH-ROBOT-PLATFORM.bash hsr
    ```

4. Pull or build the Docker images, then run the container and build the Catkin workspaces:

    ```shell
    bash ./BUILD-DOCKER-IMAGE.bash
    bash ./RUN-DOCKER-CONTAINER.bash
    openrosde-initialize-new-container
    ```

5. Exit the container (`Ctrl+d`) and open the `tmux` session of the platform:

    ```shell
    bash ./RUN-TMUX-SESSION.bash
    ```

The core needs no change to find the platform, since `bash ./SWITCH-ROBOT-PLATFORM.bash` lists every platform of the cloned extensions.
Update the platform with `git -C extensions/openrosde-platform-hsr pull`, then `bash ./scripts/update-git-submodules.bash`.

## 📖 Documentation

| Document                                                                     | Description                                                               |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| [Robot Platforms / HSR](docs/robot-platforms/hsr.md)                         | Toyota HSR (Human Support Robot) configuration, simulation, and operation |
| [Basic Usage](https://github.com/openrosde/openrosde/blob/dev/docs/basic-usage.md) | Full setup guide of the core of OpenRoSDE                                 |
| [Extensions](https://github.com/openrosde/openrosde/blob/dev/docs/extensions.md)   | How OpenRoSDE finds the files of a cloned extension and tests it          |

## 🧪 Continuous Integration

The CI of the core of OpenRoSDE tests this platform at the commit that `gitlab-ci/extensions.repos` of the core pins: it builds or reuses the image `ghcr.io/openrosde/hsr-devel`, builds the Catkin workspaces in it, and checks every launch entry point of the panes.
It publishes the image as `dev` or `main` from those branches of the core, and under the tag of a release of the core, such as `v3.0.0`.

`.github/workflows/test-with-openrosde.yml` of this repository calls the reusable workflow `extension.yml` of the core on `dev` for every push to `main` and every pull request, which checks out this commit in place of the version that the core pins, tests only the setups that run this platform, and publishes nothing.

## 🐛 Reporting Issues

Please report any issue of this platform using the GitHub issue tracker: [https://github.com/openrosde/openrosde-platform-hsr/issues](https://github.com/openrosde/openrosde-platform-hsr/issues).

## 📜 Third-Party Software

This repository holds no third-party code.
The image `hsr-devel` and the Catkin workspace bring the third-party software below, each under its own license, as [`NOTICE`](NOTICE) records.

| Component                                                                                                                                                                    | Source                                                                                                                                            | License                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `tmc_wrs_binary`: the HSR stack of ROS Noetic with its URDF, SRDF, MoveIt configuration, navigation, controllers, `hsrb_*` and `tmc_*` packages, and the WRS apartment world | First stage of the image `hsr-devel`, `ghcr.io/hsr-project/tmc_wrs_binary:noetic-latest`, made from https://github.com/hsr-project/tmc_wrs_binary | [BSD-3-Clause-Clear](https://spdx.org/licenses/BSD-3-Clause-Clear.html) (TOYOTA MOTOR CORPORATION) |
| `hsrb_meshes`: the meshes of the HSR                                                                                                                                         | Inside `tmc_wrs_binary`, from https://github.com/hsr-project/hsrb_meshes                                                                          | [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/) (TOYOTA MOTOR CORPORATION)   |
| ROS Noetic, Gazebo, FlexBE, and the other packages of the image `openrosde-base`                                                                                             | The core of OpenRoSDE, https://github.com/openrosde/openrosde                                                                                             | Per upstream, such as BSD, Apache-2.0, or MIT                                                      |

> [!WARNING]
> The meshes of the HSR in `hsrb_meshes` are licensed under CC BY-NC-ND 4.0, which allows neither commercial use nor derivatives.
> The image `hsr-devel` redistributes them inside `tmc_wrs_binary`, so a commercial use of the image needs the agreement of the rights holder.

> [!IMPORTANT]
> The image `hsr-devel` redistributes the binaries of `tmc_wrs_binary` under the Clear BSD License, whose notice `NOTICE` reproduces, as the license requires.
> Keep `NOTICE` with any copy of the image.

## 🕰️ History

The files of this platform come from the core of OpenRoSDE, [`coarobo/SDE`](https://gitlab.com/coarobo/SDE), at commit [`041d164`](https://gitlab.com/coarobo/SDE/-/commit/041d164201db41ac35db5efd7889e6424a84deca) of `dev`, where their earlier history lives.

## ⚖️ License

```
Copyright 2019 Coarobo GK

Licensed under the Apache License, Version 2.0.
```

See [`LICENSE`](LICENSE) for the license, and [`NOTICE`](NOTICE) for the MIT notice of the earlier code and the notices of third-party software.
