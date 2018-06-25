## Introduction to IoT Edge

Azure IoT Edge moves cloud analytics and custom business logic to devices so that your organization can focus on business insights instead of data management. Enable your solution to truly scale by configuring your IoT software, deploying it to devices via standard containers, and monitoring it all from the cloud.

Analytics drives business value in IoT solutions, but not all analytics needs to be in the cloud. If you want a device to respond to emergencies as quickly as possible, you can perform anomaly detection on the device itself. Similarly, if you want to reduce bandwidth costs and avoid transferring terabytes of raw data, you can perform data cleaning and aggregation locally. Then send the insights to the cloud.

In this lab you will learn about

* Deploy a Linux machine and make it an edge device

* Deploy Modules and learn how to create routes

### Deploy IoT Edge on Ubuntu 

In Azure portal click on **Create a resource** 

 ![Create Resource](../images/create_resource.png)


Select **Ubuntu** Server 

 ![Ubuntu](../images/ubuntu.png)


Create server in its own resource group. You can delete the resource group after the lab is competed.

 ![Create Ubuntu](../images/ubuntu_create.png)


Select DS1_V2 for VM Size

 ![Ubuntu VM Size](../images/ubuntu_vm_size.png)


Leave everything default

 ![Ubuntu Storage](../images/ubuntu_storage.png)
 

Click **Create** button

 ![Ubuntu Create](../images/ubuntu_create_last_step.png)

 
Got To the Resource Group created for IoT Edge and select the VM. Click **Connect** button to get the ssh command to connect to the VM.

 ![Ubuntu Connect](../images/ubuntu_connect.png)


Copy the SSH command

 ![Ubuntu SSH](../images/ubuntu_ssh.png)

 
Click on Cloud shell in the portal

 ![Cloud Shell](../images/cloud_shell.png)

 
Make sure you select **Bash** shell

 ![Cloud Shell Login](../images/cloudshell_login.png)

 
Once logged into Cloud Shell type the command 

```linux
$ ssh iotedgeadmin@ipaddress
```


You will be prompted to enter the password which you entered when creating the Ubuntu VM. Enter the password and press enter.

You will successfully login to Ubuntu VM

 ![Cloud Shell Success](../images/ubuntu_cloud_shell_success.png)


Change user to get root access by entering the following command

```linux
sudo su -
```

Check for Python version, you should have 2.7.X version.

```linux
python --version
```

Update the apt package index

```linux
apt install docker.io
```

Update the apt package index.

Check for docker version

```linux
docker --version
```

You should have a docker version installed.

## Install IoT Edge Runtime

Install pip

```linux
sudo apt-get install python-pip
```

Install IoT Edge Runtime

```linux
sudo pip install -U azure-iot-edge-runtime-ctl
```

Check IoT Edge Runtime version

```linux
iotedgectl --version
```

## Manage IoT Edge Devices using IoTHub

Create IoT Edge Device using IoT Hub. Click on **IoT Edge** from Azure Portal. 

 ![Devices](../images/iothub_iot_edge_devices.png)


Click on **+ Add IoT Edge Device**


 ![Edge Device](../images/add_iot_edge_device.png)


Add new IoT Edge device and Click **Save**

 ![device](../images/device.png)


New Edge Device is created. Click on the device

 ![Device List](../images/device_list.png)


Copy primary connection string

 ![Device Connection](../images/device_connection_string.png)


Setup Device with the following command. Make sure replace device connection string with the primary connection string you copied.

```linux
iotedgectl setup --connection-string "{device connection string}" --nopass
```

 ![Runtime Setup](../images/ubuntu_edge_runtime_setup.png)


Start the runtime with

```linux
iotedgectl start
```

 ![Runtime running](../images/iotedgectl_running.png)





Check Docker to see that the IoT Edge agent is running as a module

```linux
docker ps
```

 ![Agent](../images/iotedgectl_agent.png)

## Deploy a module

One of the key capabilities of Azure IoT Edge is being able to deploy modules to your IoT Edge devices from the cloud. An IoT Edge module is an executable package implemented as a container. In this section, you deploy a module that generates telemetry for your simulated device. 

* In the Azure portal, navigate to your IoT hub.
* Go to IoT Edge (preview) and select your IoT Edge device.
* Select Set Modules.
* Select Add IoT Edge Module.
* In the Name field, enter tempSensor. 
* In the Image URI field, enter microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview. 
* Leave the other settings unchanged, and select Save.

 ![Save Module](../images/save_module.png)


* Back in the Add modules step, select **Next**.
* In the Specify routes step, select **Next**.
* In the Review template step, select **Submit**.
* Return to the device details page and select **Refresh**. You should see the new tempSensor module running along the IoT Edge runtime.


 ![Module Running](../images/tempsensor_running.png)


On the edge device type **docker ps** to list all the modules running. You should see tempSensor module running

```linux
docker ps
```

 ![Module on Ubuntu](../images/tempsensor_on_ubuntu.png)


View logs of tempSensor module to see the data being sent to IoT Hub

```linux
docker logs -f tempSensor
```

 ![Module Data](../images/tempsensor_data.png)

