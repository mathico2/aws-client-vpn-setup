# [client-vpn-aio](../client-vpn-aio)

> ## Note: The current setup script is working but still experimental. There are many potential errors and vulnerabilities. Any help to test the script is highly welcomed! If you have used the script to deploy an endpoint, any feedback will be appreciated!

This is a python script that automatically deploys a ready-to-go OVPN endpoint in your desired AWS region. It also acts as a CLI tool to help you manage the Client VPN Endpoints. (`client-vpn-aio`, AIO stands for All-in-One)

## Requirements

Though it is a python script, there are bash commands called within the program, so it requires a Unix-like bash environment. Unfortunately, this script is not likely to be run natively in a Windows environment, since [rasy-rsa](https://github.com/openvpn/easy-rsa) can only be run in a unix-like environment. However, I think it can be run within [WSL(Windows Subsystem for Linux)](https://docs.microsoft.com/en-us/windows/wsl). The following are the prerequisites to be installed on the system:

- git
- python3
- pip
- aws cli
- boto3 (python library)

Also please note that you need to have your aws account set up with the aws cli before you can run this program. To do so, run the following command:

```shell
$ aws configure
```

## To Setup a New OVPN Endpoint.

- Clone the repo and run the python script with **_no arguments_**. However, you can append flag `-a` to the command if you want to be able to connect to the endpoint immediately after deployment.(it may take longer this way)

  ```shell
  $ cd /THE_DIR_YOU_WANT_TO_USE
  $ git clone https://github.com/scottpedia/aws-client-vpn-setup.git
  $ cd aws-client-vpn-setup
  $ ./client-vpn-aio
  ```

- You will be then prompted to enter some info about the endpoint. You can leave some of them blank and the default values will be used.

- After the endpoint is successfully created, you will see two new files under the `CWD`.

  - `xxxxx.ovpn` is the output file that you use with your OVPN client software to connect to the endpoint.

  - `xxxxx.ovpnsetup` is the file containing the properties of the deployed ovpn endpoint. This will be used to manage the OVPN endpoint it specifies.

- The endpoint is by default turned off right after the script is run. To activate the endpoint, use the management feature of the script. Run `./client-vpn-aio on` to turn on the vpn endpoint. See this [section](#To-manage-an-existing-OVPN-endpoint) for more into about the management feature.

- After the endpoint is turned on, you can connect to the endpoint with your client software. See this [section](../README.md#how-to-set-up-the-client-for-the-vpn-server) from the main article for how to setup the client software on macOS.

## To Manage an Existing OVPN Endpoint.

```
Usage: ./client-vpn-aio [command] -f [the_config_file]
The python script to deploy and manage the vpn service based on AWS Client VPN Endpoints.

NOTE: PLEASE HAVE YOUR AWS CLI SETUP WITH YOUR AWS ACCOUNT BEFORE YOU RUN THIS SCRIPT.
      THE SCRIPT WILL NOT RUN WITHOUT AN AWS ACCOUNT SETUP WITH THE CLI.

***TO DEPLOY A NEW VPN SERVICE, please run the script without any command.***

    -a
    Add this flag to indicate that you want the endpoint to be enabled right after a successful deployment.
    By default, the endpoint is off upon a successful deployment.

***TO MANAGE AN EXISTING ENDPOINT, please use the following commands:***
    status    :   Output the current status of the specified VPN Endpoint.
    on        :   Turn on the VPN
    off       :   Turn off the VPN
    toggle    :   Toggle the VPN
    terminate :   Terminate the selected OVPN setup on AWS.
   *help      :   Output the help

    -f [Filename] (Optional)
    You can use the optional -f flag to specify the file which contains the profile of a specific VPN deployment(*.ovpnsetup).
    Thus you can have multiple deployments active at the same time, and manage each of them with its profile.
    If the file is not speficied, the program will automatically look for one under the current working directory.
    If multiple profiles are found under the CWD, should the most recent one be used.
```
