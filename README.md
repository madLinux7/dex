# dockerExec (dex)

[![Shell](https://img.shields.io/badge/shell-bash-4EAA25?style=flat-square&logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![License](https://img.shields.io/github/license/madLinux7/dex?style=flat-square)](https://github.com/madLinux7/dex/blob/main/LICENSE)

A dead-simple, quality-of-life CLI booster for quick **Docker (Compose)** container access.

Save time by running commands in containers dynamically resolved from your local directory configuration.

- **Context-Aware** — Remembers configuration per-directory via a local `.dex` file.
- **Docker Compose Mode** — Resolves container IDs dynamically from a service name (e.g. `app`) and compose file, avoiding stale IDs.
- **Direct Container Mode** — Executes directly against a fixed container name if Docker Compose isn't used.
- **Compose Shortcuts** — Built-in support for `--up`, `--down`, and `--restart` directly inside your target project.
- **Interactive Shells** — Running `dex` without commands automatically spins up an interactive shell inside the container (defaults to `sh`, configurable in `.dex`).

## Usage

```
dex -a, --assign [container_name]                  Assign a single container name
dex -a, --assign [compose_file] [service_name]     Assign a service in a docker-compose file
dex -c, --clear                                    Clear the local dex assignment
dex --up [params]                                  Run docker compose up (compose mode only)
dex --down [params]                                Run docker compose down (compose mode only)
dex --restart [params]                             Restart the assigned container/service
dex [COMMANDS]                                     Run commands inside the assigned container
dex                                                Start an interactive shell in the assigned container
```

### 1. Assigning a Container

#### Compose Mode
To assign a Docker Compose service in the current directory:
```bash
dex -a docker-compose.dev.yml app
```

#### Direct Container Mode
To assign a single running container by it's name:
```bash
dex -a cool-container-7
```

### 2. Running Commands
Execute any command inside the container:
```bash
dex php artisan migrate
```

### 3. Interactive Shell
Run `dex` with no arguments to open an interactive terminal:
```bash
dex
```

### 4. Compose Commands (Compose Mode Only)
Manage your Compose stack from the assigned directory:
```bash
dex --up -d
dex --down --remove-orphans
dex --restart
```

### 5. Cleaning Up
Remove the local directory configuration:
```bash
dex -c
# or
dex --clear
```

## Configuration

The configuration is saved in the current directory inside a `.dex` file:

### Compose file assignment

```sh
MODE="compose"
COMPOSE_FILE="docker-compose.dev.yml"
SERVICE_NAME="app"
SHELL="sh"
```

### Container assignment

```sh
MODE="container"
CONTAINER_NAME="cool-container-7"
SHELL="sh"
```

## Install / Update script

```bash
curl -fsSL https://artifacts.grolmes.com/dex/install.sh | sh
```

### Build/Install from Source

Clone the repository and copy the script to your local bin directory:

```bash
git clone https://github.com/madLinux7/dex.git
cd dex
chmod +x dex
cp dex ~/.local/bin/dex
```