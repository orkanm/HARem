# Environment Installation & Setup Guide

This guide will walk you through setting up your local development environment for the **HARem** project.

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Python 3.11+**: The project is tested with recent Python versions.
- **python3-venv**: Required for creating isolated Python environments.
  - **On Ubuntu/Debian**:
    ```bash
    sudo apt update && sudo apt install -y python3-venv
    ```

## Step-by-Step Setup

### 1. Create a Virtual Environment

It is highly recommended to use a virtual environment to avoid dependency conflicts.

```bash
# Navigate to the project root
cd HARem

# Create the virtual environment
python3 -m venv esphome_venv
```

### 2. Activate and Install Requirements

Activate the environment and install the necessary Python packages (including ESPHome).

```bash
# Activate the environment
source esphome_venv/bin/activate

# Install requirements
pip install -r requirements.txt
```

### 3. Configure Secrets

The project uses a `secrets.yaml` file for sensitive information like Wi-Fi credentials and API keys. This file is excluded from Git.

1.  **Copy the template**:
    ```bash
    cp secrets_example.yaml secrets.yaml
    ```
2.  **Generate an API Key**:
    ESPHome requires a base64-encoded 32-byte key for API encryption. You can generate one using:
    ```bash
    openssl rand -base64 32
    ```
3.  **Edit `secrets.yaml`**:
    Open `secrets.yaml` in your editor and fill in the following:
    - `wifi_ssid`: Your Wi-Fi name.
    - `wifi_password`: Your Wi-Fi password.
    - `harem_api_key`: The base64 key generated in the previous step.
    - `harem_ota_password`: A password for Over-The-Air updates.

### 4. Validate Configuration

To ensure everything is set up correctly without needing hardware connected, run the validation command:

```bash
esphome config remote_controller.yaml
```

If you see `INFO Configuration is valid!`, you are ready to proceed.

## Building and Flashing

Once your hardware (ESP32-C3) is connected via USB:

```bash
# Compile and flash to the device
esphome run remote_controller.yaml
```

## Troubleshooting

- **Missing `python3-venv`**: If you get an error about `ensurepip`, run `sudo apt install python3-venv`.
- **Invalid API Key**: Ensure the `harem_api_key` in `secrets.yaml` is a valid base64 string.
- **Port Permission**: If you cannot flash, you may need to add your user to the `dialout` group:
  ```bash
  sudo usermod -a -G dialout $USER
  ```
  (Requires logout/login to take effect).
