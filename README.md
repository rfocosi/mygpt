# Installing Ollama and Open WebUI

This document provides a step-by-step guide on how to install Ollama and Open WebUI using Docker.

## Prerequisites

*   A Linux-based operating system (Ubuntu, Debian, etc.) OR Windows WSL2
*   Docker installed and running
*   Nvidia drivers installed
*   Basic command-line knowledge

## Installation Steps

### 1. Pull images

This command will pull all required images in our Docker compose.

```bash
# docker compose pull
```

### 2. Run Ollama

Ollama is a lightweight framework for running large language models locally.

```bash
# docker compose up -d ollama
```

This command will run and create, when first time running, the directories to keep ollama configurations.

Check if Ollama is running and accessible:

```bash
# curl http://localhost:11434/
```

Will return: `Ollama is running`

#### 2.1. GPU Check

To verify if your GPU was detected, run:

```bash
# docker compose logs ollama
```

And search for (example):

```
ollama  | time=2025-06-19T17:09:30.103Z level=INFO source=gpu.go:217 msg="looking for compatible GPUs"
ollama  | time=2025-06-19T17:09:30.338Z level=INFO source=types.go:130 msg="inference compute" id=GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx library=cuda variant=v12 compute=8.6 driver=12.8 name="NVIDIA GeForce RTX 3060" total="11.6 GiB" available="11.5 GiB"
```

#### 2.2. Install a model

This command will access the running Ollama container:

```bash
# docker compose exec -it ollama bash
```

To find a model, access the oficial Ollama library: [ollama.com/library](https://ollama.com/library)

To install a model, run:

```bash
# ollama pull <model_name>
```

For example, to install the `llama2` model:

```bash
# ollama pull llama2:7b
```

To list installed models:

```bash
# ollama list
NAME         ID              SIZE      MODIFIED       
llama2:7b    78e26419b446    3.8 GB    17 seconds ago
```

If you want to give it a run:

```bash
# ollama run llama2
>>> How to raise chickens?
```

Type `/bye` to exit.

To exit the container, type `exit`.

### 3. Run Open WebUI

Open WebUI is a user interface for interacting with Ollama models.

```bash
# docker compose up -d open-webui
```

This command will run the Open WebUI Docker image, exposing the web interface on port 8080.

Wait for startup, checking the logs:

```bash
# docker compose logs -f open-webui
```

#### 3.1. Access Open WebUI

Once the Docker container is running, you can access Open WebUI by navigating to `http://localhost:8080` in your web browser.

In the first access, it will ask to create an **Administrator Account**.

Add the credentials and continue.

### 4. Download and Run a Model

Within the Open WebUI interface, you can download and run various models. For example, to download and run the Llama 2 model:

1. Access '**Admin Panel**'
2.  Click on the '**Connections**' tab
    * Disable '**Open API**' and '**Direct Connections**'
    * Enable '**API Ollama**'
    * In '**Manage Ollama API Connections**', verify if address is: `http://host.docker.internal:11434`
        * You can check the Ollama API connection, clicking on '**Configure**' and after on '**Verify Connection**' icons.
3.  Click on the '**Models**' tab.
    * Search for '**llama2**'.

If '**llama2**' is available, you can navigate to '**New chat**' and try it out: '**How to raise chickens?**'

## Troubleshooting

*   If you encounter issues with Docker, ensure that Docker is properly installed and running on your system.
*   If you cannot access Open WebUI, check if the Docker container is running and that port 8080 or port 11434 is not being used by another application.

## Further Information

*   Ollama Documentation: [https://ollama.com/](https://ollama.com/)
*   Open WebUI Documentation: [https://docs.openwebui.com/](https://docs.openwebui.com/)
