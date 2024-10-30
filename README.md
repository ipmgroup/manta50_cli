# Manta Controller Parameter Manager

This script is designed for managing parameters of **Manta50** and **Manta100** controllers via the **DroneCAN** interface. It allows users to read and write parameters, change Node ID, telemetry rate, CAN speed, and other settings.

## Requirements

- Python 3.x
- Libraries: `dronecan`, `argparse`, `json`

## Setup

1. Ensure the CAN interface (`can0`) is configured and active.
2. Connect your Manta controller (Manta50 or Manta100) to the CAN network.
3. Install the required Python dependencies:

   ```bash
   pip install dronecan argparse json
   ```

## Usage

The script supports two modes:
- **Reading parameters** (`read`, default mode): Reads all controller parameters and saves them to a JSON file.
- **Writing parameters** (`write`): Sends settings to the controller.

> **Note**: In `write` mode, the `--esc_index` argument is required. The `--node_id` argument is optional if only one controller is connected to the CAN network. If there are multiple controllers on the network, specify `--node_id` to select the desired node.

### Command-Line Arguments

| Argument            | Description                                                                                   |
|---------------------|-----------------------------------------------------------------------------------------------|
| `--mode`            | Operation mode: `read` for reading parameters (default), `write` for writing parameters.      |
| `--node_id`         | Current Node ID of the controller. Optional if there is only one controller in the network.   |
| `--set_node_id`     | New Node ID to be written to the controller’s parameters.                                     |
| `--esc_index`       | ESC index (required in `write` mode).                                                         |
| `--filename`        | Filename to save/load parameters, e.g., `manta_params.json`.                                  |

### Usage Examples

#### Reading Parameters

To retrieve the current parameters of the controller and save them to a JSON file:

```bash
python param_to_file.py --filename manta_params.json
```

This will print the node's parameters to the console and save them in `manta_params.json`. If multiple controllers are connected, specify `--node_id` to select the correct node.

#### Writing Parameters

Use `write` mode to modify controller parameters. **The `--esc_index` argument is mandatory in write mode**. 

1. **Changing Node ID**:

   ```bash
   python param_to_file.py --mode write --esc_index 0 --set_node_id 1
   ```

   Here, `--esc_index` specifies the ESC index, and `--set_node_id` is the new Node ID to be written to the controller.

2. **Writing parameters from a file**:

   ```bash
   python param_to_file.py --mode write --esc_index 0 --set_node_id 1 --filename manta_params.json
   ```

   This loads parameters from `manta_params.json` and sends them to the controller.

### Script Functionality

1. **`read` Mode**: Reads and saves the controller's parameters to a JSON file. Useful for backing up parameters before making changes.
2. **`write` Mode**: Sends new parameters to the controller using values from a JSON file or command-line arguments. The script restarts the node to apply the changes.

## Notes

- If `--node_id` is not specified, the script will try to determine it from the only available node on the CAN network.
- **Both** `--node_id` **and** `--esc_index` **are required if multiple controllers are connected to the network**.
