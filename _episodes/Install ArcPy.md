---
title: "Install ArcPy to Palmetto JHub"
teaching: 5 min
exercises: 0
questions: "How to install Arcpy to Palmetto"
objectives:

keypoints:
- "ArcPy"
---


In order to install arcpy, one need to install ArcGIS Server. User need to be added to ESRI account in order to be authorized to run arcpy.

Following are the steps to install arcpy with python 3.6 using conda environment.

## Step 1. Create a license file:

Go to https://my.esri.com/ and login using Clemson username/password. Once done, navigate to My Organization and click on Licensing

![image](https://user-images.githubusercontent.com/43855029/123677715-74c55600-d813-11eb-8584-4e2f73a6578c.png)

Start Licensing Esri Product: and select the following option:

![image](https://user-images.githubusercontent.com/43855029/123677818-932b5180-d813-11eb-8e6d-1aaa95dd20a5.png)

Click Next and Select “ArcGIS Image server” and Next

Enter your own information

Download your license file and upload to Palmetto: For example: ArcGISImageServer_ArcGISServer_1007035.prvc

![image](https://user-images.githubusercontent.com/43855029/123677857-9cb4b980-d813-11eb-961f-857840128621.png)

## Step 2. Download ArcGIS Server:

Download and upload to Palmetto.

![image](https://user-images.githubusercontent.com/43855029/123677914-ab02d580-d813-11eb-81e9-bf6721093b51.png)

## Step 3. Setup ArcGIS Server:

SSH to the node for our team:

```bash
$ ssh -X node2065
```

Navigate to installation folder and run Setup file:

```bash
$ ./Setup
```

## Step 4. Authorize the license file:

There are 2 ways to authorize the license file from step 1.

### Method 1: Using GUI:

- Request a compute node with GUI
- Go to /home/$USER/arcgis/server/tool
- Run authorizeSoftware in GUI and select the ArcGISImageServer_ArcGISServer_1007035.prvc from your directory. Check your information and authorize it.
- Make sure that all tick marks are checked for the authorization:
- Testing by running:

```bash
$ ./authorizeSoftware -s
```

### Method 2: using silent mode.

```bash
$ ./ authorizeSoftware <-f .prvc> <-e email> <-o filename.txt>
```

Upload the filename.txt to esri website (following its instruction) to obtain the license file: authorization.ecp
Validate the license:

```bash
$ ./authorizeSoftware -f authorization.ecp
```

Make sure it works:

```bash
$ ./authorizeSoftware -s
```

The GUI appears for you to manually install ArcGIS Server to your /home/username/arcgis directory


## Step 5. Install conda environment:

Request a compute node without -X

```bash
$ module load anaconda3/5.1.0-gcc/8.1.0
$ conda create -n arcpy_env -c esri arcgis-server-py3=10.8.1
$ export ARCGISHOME=/home/$USER/arcgis/server
$ source activate arcpy_env
$ import arcpy
```

Test to make sure it works

## Step 6. Create Jupyter Kernel:

```bash
$ source activate arcpy_env
$ conda install -y -c conda-forge kernda
$ python -m ipykernel install --user --name arcpy_env --display-name "MyArcPy"
$ kernda /home/tuev/.local/share/jupyter/kernels/arcpy_env/kernel.json -o
```

Modify /home/tuev/.local/share/jupyter/kernels/arcpy_env/kernel.json and make sure the following lines are added:

```bash
{
  "argv": [
    "bash",
    "-c",
    "source \"/software/spackages/linux-centos8-x86_64/gcc-8.3.1/anaconda3-5.1.0-c3p5et4cpo7jaiahacqa3pqwhop7tiik/bin/activate\" \"/home/tuev/.conda/envs/arcpy1\" && exec /home/tuev/.conda/envs/arcpy1/bin/python -m ipykernel_launcher -f '{connection_file}' "
  ],
  "env": {"ARCGISHOME":"/home/tuev/arcgis/server"},
  "display_name": "MyArcPy510",
  "language": "python"
}
```

Start JupyterHub, load the kernel and try importing arcpy



