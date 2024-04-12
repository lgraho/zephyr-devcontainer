# Zephyr VS Code Dev Container

This is a simple template for working with Zephyr in a VS Code Dev Container.

All required tools, packages and the Zephyr SDK are already setup within the container (see https://docs.zephyrproject.org/latest/develop/getting_started/index.html for details).

## Project Structure

Zephyr supports different [application types](https://docs.zephyrproject.org/latest/develop/application/index.html#application-types). In this project we use a [freestanding application](https://docs.zephyrproject.org/latest/develop/application/index.html#zephyr-freestanding-app). In a freestanding application, the Zephyr application code (`app/`) is located outside the Zephyr workspace (`zephyrproject/`):

```
/home/user/workspace/
├─── zephyrproject/
│     ├─── .west/
│     │    └─── config
│     ├── zephyr/
│     ├── bootloader/
│     ├── modules/
│     └── ...
│
└─── app/
     ├── CMakeLists.txt
     ├── prj.conf
     └── src/
         └── main.c
```

The Zephyr workspace (`zephyrproject/` folder) is populated during setup of the container (see [Initialize Zephyr](#initialize-zephyr)).

The project already contains a minimal compilable application under `app/`.

## Prerequisites

(see https://code.visualstudio.com/docs/devcontainers/containers for a detailed guide about dev containers)

- Visual Studio Code with the following extensions:
  - Docker
  - Dev Containers
  - WSL (on Windows)
- Docker
- WSL2

## Getting Started

### Clone Repository

```
git clone https://github.com/lgraho/zephyr-devcontainer
```

### Open Workspace

1. Open the `zephyr-devcontainer.code-workspace` file with VS Code.
2. Use the **Dev Containers: Reopen in Container** command to open the workspace in a new container (press **F1** and type **Dev Containers: Reopen in Container**). On first setup this may take a while.

### Initial Setup

On initial setup of the container, a Zephyr project needs to be initialized and the required Python dependencies need to be installed in the container.

This is done with the following commands. Alternatively, there are also already predefined VS Code tasks for these steps defined in `.vscode/tasks.json`.

#### Initialize Zephyr

```
west init ~/workspace/zephyrproject
cd ~/workspace/zephyrproject
west update
```

OR

1. Press **Ctrl + P** and type `task west init`.
2. Press **Ctrl + P** and type `task west update`.

#### Export Zephyr CMake package

```
cd ~/workspace/zephyrproject/zephyr
west zephyr-export
```

OR

Press **Ctrl + P** and type `task west zephyr-export`.

#### Install Python dependencies

```
pip3 install --user -r ~/workspace/zephyrproject/zephyr/scripts/requirements.txt
```

OR

Press **Ctrl + P** and type `task install dependencies`.

## Build

The project already contains a minimal application under `app/src` which can be built using the `west build` command which simply sets up the build directory and invokes CMake.

The application can be built using the predefined `build` task by pressing **Ctrl + Shift + B** (update the board parameter in `task.json` as required), or via the following command:

```
cd ~/workspace/app
west build -b <BOARD> -d ../build
```

Replace `<BOARD>` with your board (e.g. `rpi_pico`; see [Supported Boards](https://docs.zephyrproject.org/latest/boards/index.html)).

### With CMake

Alternatively, one can also use CMake directly: 

```
cd ~/workspace/app
cmake -B ../build -DBOARD=<BOARD>
```

## Useful Resources

The following resources helped setting up this project:

- https://docs.zephyrproject.org/latest/develop/getting_started/index.html
- https://github.com/zephyrproject-rtos/docker-image
- https://github.com/embeddedcontainers/zephyr
- https://interrupt.memfault.com/blog/practical_zephyr_basics
