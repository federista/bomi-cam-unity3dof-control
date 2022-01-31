# biorob_group12_bomi

Meta-repository containing both the python scripts (used for 'target reaching', 'mouse control' and 'unity mechanism control') and the nodejs server used for interfacing the scripts with Unity3D.

## Running locally

## Running using docker and docker compose

You will need to install `docker-compose` if it's not present on your system already (see [install docker-compose](https://docs.docker.com/compose/install))

Once you have `docker-compose` installed you will just have to run the compose file present in this repo with

```bash
.../biorob_group12_bomi$ docker-compose up
```

This will launch the dockerfiles

# Unity Files

In the directory `unity-docker-example` are stored the bare minimum files needed to execute the Unity3D WebGL client (i.e., the robotic arm controlled by the joints). The files have been generated starting from the project presented in the [kTonpa/3dof-robot-arm-using-unity3D](https://github.com/kTonpa/3dof-robot-arm-using-unity3D) repository, although the entire project is not stored on GitHub to lower the space requirements of this repository. However do not hesitate to open an issue (or contact me in any other way you deem more convoluted and therefore funnier).
The app is launched using a simple nginx container (see forked repo).