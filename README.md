# OPC Twin Services

![Build Status](https://msazure.visualstudio.com/_apis/public/build/definitions/b32aa71e-8ed2-41b2-9d77-5bc261222004/33977/badge)

The Twin micro services contained in this repository provide a REST front end for OPC UA application, endpoint, and module identity management and control using the Azure IoT Hub Device Twin service.  

The **OPC UA twin registry micro service** provides access to registered OPC UA applications and their endpoints.  Operators and administrators can register and unregister new OPC UA applications and browse the existing ones, including their endpoints.  

In addition to application and endpoint management, the registry service also catalogues registered [OPC Twin IoT Edge modules](https://github.com/Azure/azure-iiot-opc-twin-module).  The service API gives you control of edge module functionality, e.g. starting or stopping Server discovery (Scanning services), or activating new 'Endpoint twins' that can be accessed using the OPC Twin micro service.

The **OPC UA twin micro service** facilitates communication with factory floor edge OPC UA server devices via a [OPC Twin IoT Edge module](https://github.com/Azure/azure-iiot-opc-twin-module) and exposes OPC UA services (Browse, Read, Write, and Execute) via its REST API.  

The **onboarding agent service** is a worker role that processes OPC UA server discovery events sent by the [OPC Twin IoT Edge module](https://github.com/Azure/azure-iiot-opc-twin-module) when in discovery (or scan) mode.  The discovery events result in application registration and updates in the OPC Twin Registry.

The OPC Twin services are part of our suite of [Industrial IoT (IIoT) solution accelerator components](#Other-Industrial-IoT-components).

## Getting started

Clone this repository.

### Install any tools and depdendencies

* [Install .NET Core 2.1+][dotnet-install] if you want to build the services locally.
* Install [Docker][docker-url] if you want to build and run the services using docker-compose.
* Install Powershell to deploy dependent services in Azure using the deployment scripts.
* Install any recent edition of Visual Studio (Windows/MacOS) or Visual Studio Code (Windows/MacOS/Linux).
   * If you already have Visual Studio installed, then ensure you have [.NET Core Tools for Visual Studio 2017][dotnetcore-tools-url] installed (Windows only).
   * If you already have VS Code installed, then ensure you have the [C# for Visual Studio Code (powered by OmniSharp)][omnisharp-url] extension installed. 

### Deploy required Azure Services

This service has at a minimum a dependency on the following Azure resources:

* [Azure IoT Hub][iothub-docs-url]
* [Azure Storage][storage-docs-url]

If you have [PowerShell](#Install-any-tools-and-depdendencies) installed on your machine you can deploy them using the deploy.ps1 script contained in the deploy folder of the repo.  Run `./deploy.ps1 -type local` and choose to save the resulting .env file.

### Setup Environment variables

In order to run the services, some environment variables need to be created at least once. More information on environment variables [below](#Configuration-And-Environment-Variables).  

If you deploy all dependencies using [PowerShell](#Deploy-required-Azure-Services), the resulting .env file or script output shows all the variables the services expect.  Export each, or source them into your development environment before starting any service locally.  If you run the services using docker-compose, no further action is required.

## Build and Run

### Running all services using docker

If you have [Docker](#Install-any-tools-and-depdendencies) installed, you can start all services by changing into the repo root and running `docker-compose up`.  This requires that all needed environment variables were written into the .env file by the deployment script.

To start the OPC Twin module follow the instructions [here](https://github.com/Azure/azure-iiot-opc-twin-module).

> For real world usage, run one or more OPC UA servers in the same network that the above module deployment is part of.

### Build and run the service with Visual Studio or VS Code

1. Make sure the [Prerequisites](#Install-any-tools-and-depdendencies) are set up.
1. Open the solution in Visual Studio or VS Code
1. Configure the solution start up projects as `Microsoft.Azure.IIoT.OpcUa.Services.Twin`, `Microsoft.Azure.IIoT.OpcUa.Services.Onboarding`, and `Microsoft.Azure.IIoT.OpcUa.Services.Registry`
1. Start all projects (e.g. press F5).
1. Open a browser to `http://localhost:9041/` and `http://localhost:9042/` and test the each REST API using the services' Swagger UI or the [OPC Twin CLI](https://github.com/Azure/azure-iiot-opc-twin-api).

### Configuration and Environment variables

Each service can be configured in its [appsettings.json](src/appsettings.json) file.  Alternatively, all configuration can be overridden on the command line, or through environment variables.  If you have deployed the dependent services using [PowerShell](#Install-any-tools-and-depdendencies), make sure the environment variables shown at the end of deployment are all persisted in your environment.

* [This page][windows-envvars-howto-url] describes how to setup env vars in Windows.
* For Linux and MacOS, we suggest to create a shell script to set up the environment variables each time before starting the service host (e.g. VS Code or docker). Depending on OS and terminal, there are ways to persist values globally, for more information [this](https://stackoverflow.com/questions/13046624/how-to-permanently-export-a-variable-in-linux), [this](https://help.ubuntu.com/community/EnvironmentVariables), or [this](https://stackoverflow.com/questions/135688/setting-environment-variables-in-os-x) page should help.

> Make sure to restart your editor or IDE after setting your environment variables to ensure they are picked up.

## Other Industrial IoT components

* OPC Twin common business logic (Coming soon)
* [OPC Twin API](https://github.com/Azure/azure-iiot-opc-twin-api)
* [OPC Twin IoT Edge module](https://github.com/Azure/azure-iiot-opc-twin-module)
* [OPC Publisher IoT Edge module](https://github.com/Azure/iot-edge-opc-publisher)
* OPC Vault service (Coming soon)

## Contributing

Refer to our [contribution guidelines](CONTRIBUTING.md).

## Feedback

Please enter issues, bugs, or suggestions as GitHub Issues [here](https://github.com/Azure/azure-iiot-opc-twin-service/issues).

## License

Copyright (c) Microsoft Corporation. All rights reserved.
Licensed under the [MIT](LICENSE) License.

[run-with-docker-url]: https://docs.microsoft.com/azure/iot-suite/iot-suite-remote-monitoring-deploy-local#run-the-microservices-in-docker
[rm-arch-url]: https://docs.microsoft.com/azure/iot-suite/iot-suite-remote-monitoring-sample-walkthrough
[postman-url]: https://www.getpostman.com
[iotedge-url]: https://github.com/Azure/iotedge
[iothub-docs-url]: https://docs.microsoft.com/azure/iot-hub/
[storage-docs-url]: https://docs.microsoft.com/en-us/azure/storage/
[docker-url]: https://www.docker.com/
[dotnet-install]: https://www.microsoft.com/net/learn/get-started
[vs-install-url]: https://www.visualstudio.com/downloads
[dotnetcore-tools-url]: https://www.microsoft.com/net/core#windowsvs2017
[omnisharp-url]: https://github.com/OmniSharp/omnisharp-vscode
[windows-envvars-howto-url]: https://superuser.com/questions/949560/how-do-i-set-system-environment-variables-in-windows-10
[iothub-connstring-blog]: https://blogs.msdn.microsoft.com/iotdev/2017/05/09/understand-different-connection-strings-in-azure-iot-hub/
[deploy-rm]: https://docs.microsoft.com/azure/iot-suite/iot-suite-remote-monitoring-deploy
[deploy-local]: https://docs.microsoft.com/azure/iot-suite/iot-suite-remote-monitoring-deploy-local#deploy-the-azure-services
[disable-auth]: https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#disable-authentication
