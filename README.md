---
page_type: sample
languages:
- docker
- docker-compose
products:
- azure
- azure-iot-hub
- azure-iot-edge
description: "This is the TICK stack sandbox, converted for Azure IoT Edge"
urlFragment: "az-iot-edge-tick"
---

# TICK on the Azure IoT Edge

This is the [TICK stack sandbox](https://github.com/influxdata/sandbox), converted for Azure IoT Edge.

## Contents

| File/folder       | Description                                |
|-------------------|--------------------------------------------|
| `docker-compose.yml`             | The docker-compose file that builds and runs this solution.                        |
| `deployment.template.json`             | The IoT Edge deployment manifest template that generates the solution manifest.                        |
| `.env`             | The solution .env file that holds all the solution environment variable values                         |
| `modules`             | The modules that this solution contains.                        |
| `config`             | The configuration that the solution modules will use during the container image build step.                        |
| `.gitignore`      | Define what to ignore at commit time.      |
| `CONTRIBUTING.md` | Guidelines for contributing to the sample. |
| `README.md`       | This README file.                          |
| `LICENSE`         | The license for the sample.                |

## Prerequisites

This is an Azure IoT Edge application that you can build and publish using VS Code with the IoT Edge extensions. For more information on how to use Visual Studio Code to develop and debug modules for Azure IoT Edge you can read [here](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-vs-code-develop-module).

## To build and push and deploy

There are two different ways to build push and deploy this stack on the edge:

### 1. Using `docker-compose` and `az cli`

You can use the provided `docker-compose.yml` file to build and push the module containers:

Clone this repo:

``` bash
git clone https://github.com/paloukari/az-iot-edge-tick
cd az-iot-edge-tick
```

Edit .env file to set your container registry:

``` bash
vim .env
```

Build and push the containers:

``` bash
docker-compose build
docker-compose push
```

Edit the generated deployment manifest to match the pushed container names, registry and registry credentials:

``` bash
vim config/deployment.amd64.json
```

Push the manifest:

``` bash
az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./config/deployment.amd64.json
```

### 2. Using VS Code

This repo is compatible with the VS Code Azure IoT Edge extension structure.

1. Clone this repo and open the project:

``` bash
git clone https://github.com/paloukari/az-iot-edge-tick
cd az-iot-edge-tick
code .
```

> Make sure you have the **Azure IoT Edge extension** installed.

1. Edit .env file and set your container registry and credentials.

1. Hit F1 and type Azure IoT Edge: *Generate IoT Edge deployment manifest*
  
1. Hit F1 and type Azure IoT Edge: *Build and push IoT Edge Solution*

1. In the Azure IoT Hub VS Code pane, right click on a device and select *Create deployment for single device*

## Fire up the IoT Edge Device on your local environment and test the TICK stack

Get a device connection string and run

``` bash
docker run -it -d --privileged -p 8888:8888 -e connectionString='YOUR DEVICE CONNECTION STRING' toolboc/azure-iot-edge-device-container
```

> This command will deploy and start the TICK stack inside a single container

Launch Chronograf at: http://localhost:8888

> To see real time docker diagnostics, navigate to the hosts list and open the *telegraf-getting-started*

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
