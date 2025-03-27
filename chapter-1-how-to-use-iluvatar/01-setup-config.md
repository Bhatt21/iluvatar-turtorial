### Config Setup for Iluvatar Worker Node
In this section, you'll configure a worker node for Iluvatar using its HTTP interface. 
We'll walk through the key configuration properties and adjustments you need to make.

## Overview of the Configuration
Your configuration file (typically in JSON format) contains several sections.
The config file is present in iluvatar-faas/src/Iluvatar/src/worker.json of the repository.
 For this tutorial, pay special attention to:

http_server: Defines the HTTP interface details.

logging: Configure logging to output to stdout.

networking: Set up the correct hardware interface.

##  Adjusting the Configuration
# Enable HTTP Interface for Easy Interaction
For the purpose of this tutorial, we'll use the HTTP interface instead of RPC. Check the http_server section and make sure it is enabled, with the proper address and port:
  - Address: 127.0.0.1
  - Port: 8080 (can by anything chosen by you)

# Direct Server Logs to stdout
For easier debugging and visibility, configure the logging section to print logs to stdout.
You can do this by adding a new property "stdout": true within the logging section:
```json
"logging": {
  "level": "info",
  "directory": "/tmp/iluvatar/logs",
  "basename": "worker",
  "spanning": "NEW+CLOSE",
  "flame": "",
  "span_energy_monitoring": false,
  "stdout": true 
}
```

# Set the Correct Hardware Interface
The networking section contains a hardware_interface property. 
This should match your machine’s actual network interface name. To check your hardware interfaces, run:
```bash
ifconfig
```
or, on some systems:
```bash
ip addr
```
Look for the interface that connects you to your network (commonly named eth0, ens33, etc.) and update the property accordingly:
```json
"networking": {
    ...
  "hardware_interface": "your_interface_name"
}
```
Replace your_interface_name with the actual interface you identified.

# Running the Worker Node
Once you have updated your configuration file (let’s say you saved it as config.json), run the Iluvatar worker node using:
```bash
sudo ./<path_to_iluvatar_worker_binary> -c <path_to_worker.json>
```
If there are no errors during startup, your worker is configured correctly and ready to run functions!