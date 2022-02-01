# bomi-cam-unity3dof-control

Meta-repository containing both the python scripts (used for 'target reaching', 'mouse control' and 'unity mechanism control') and the nodejs server used for interfacing the scripts with Unity3D.

## Team Members

_In alphabetic order:_

| Name | email  | profile |
| :--- | :---   | :--- |
| Federico Danzi | s5066568@studenti.unige.it | [Etruria89](https://github.com/Etruria89)
| Marco Gabriele Fedozzi | s5083365@studenti.unige.it | [hypothe](https://github.com/hypothe)
| Amanzhol Raisov | s4889656@studenti.unige.it | [robotmiro1](https://github.com/robotmiro1)
| Laura Triglia | s4494106@studenti.unige.it | [lauratriglia](https://github.com/lauratriglia)

![3dof-arm](https://raw.githubusercontent.com/hypothe/bomi-cam-unity3dof-control/main/.github/images/3dof_static.jpg)



The scripts presented in the two submodules are combined here (see the **docker-compose** section) to control a 3dof robotic arm in a Unity3D simulation by mapping relevant joint positions of the user - in the 2D image frame detected using the pc webcam - to the angular position of each of the robot joint (_direct kinematic position control_).


# Running using docker and docker compose

You will need to install `docker-compose` if it's not present on your system already (see [install docker-compose](https://docs.docker.com/compose/install))

Once you have `docker-compose` installed you will just have to run the compose file present in this repo with

```bash
.../bomi-cam-unity3dof-control$ docker-compose up
```

This will launch two containers, one for each of the two submodules in this repo.

Once both containers are up (it might take a while to build them the first time), open your web browser of choice and go to

`http://localhost:8080`.

You should see the Unity3D logo and a progress bar loading, after which the simulation should appear.

> both _Mozilla Firefox_ and _Google Chrome_ have been tested and working

## bomi_server

The Node.JS server that receives information from the python client and hosts the WebGL Unity3D scene (see [the original repo](https://github.com/hypothe/bomi_fama_nodejs)) for a more in-depth explanation).

## bomi_client

The python client that is used for motion tracking via a webcam (or a _prerecorded video_, the only option os Windows systems at the moment). Has a simple GUI and uses [mediapipe](https://google.github.io/mediapipe/) for the tracking.
Note that two different applications are implemented:
- the first in which the user motion is used to control either a circle in a dedicated pygame GUI or the mouse cursor on the screen (file: `main_reaching.py`)
- the other one which instead uses the motion information to control the 3dof mechanism spawn in a Unity3D scene (by the other docker).

Please note that the docker-compose automatically spawns the latter, as the former is intended to be run as a standalone script (see [the original repo](https://github.com/hypothe/markerlessBoMI_FaMa)) for a more in-depth explanation).

---

**IMPORTANT**: if you are running the command in Unix you have to add the docker to the list of services allowed to access the xserver **before** the docker-compose, or else you will be prompted with an error right after the containers spawn. In order to do that you can simply type in the shell:

```bash
$ xauth +local:docker
```

If, instead, you are using WSL:
- if you have one of the latest Windows build, with WSLG included, you have nothing else to do, it will autonomously manage the GUI
- otherwise you might have to use a third-party XServer (e.g., [XMing](https://sourceforge.net/projects/xming/))

# Running locally

The various parts of the project - Node.JS server, WebGL application, Python script - communicate using a websocket, and all routing is taken care of by the docker-compose, and that's the recommanded way of testing the project. If you prefer to instead run this locally, beware you might require a few different libraries, that you can find by looking at the Dockerfiles inside each repository. Take also 

---

# Known Issues

## Running on Windows

The current project was designed and tested mostly on Unix systems, including Windows' WSL. The functions used in the libraries (mostly regarding python) should be platform independant, but full compatibility is not guaranteed to work.
However, plese take into account that, at the time of writing this, WSL does **not** support accessing to USB devices (rather, they're midway through it). What this means is that, even if this project can run on WSL, you need to use a *pre-recorded video* instead of the live feed from the webcam in all steps **unless** you run it locally instead of using the suggested docker-compose.

The video format should be `.mp4`, `.avi`, `.mkv` or any other of the most common video file formats. It's enough to place it inside your local copy of the repository, **inside the markerlessBoMI_FaMa** folder, before spinning up the docker-compose.
You can find, at the bottom of the Python-Client GUI window that pops up, a text box with a `Ext. Video Source` button: clicking on it you will be prompted to select the video file that you wish to use as the source. You can then use it for the calibration, customization and practice, or you can use different videos across those sections (selecting them as you did for the first one).

> Note: using the external video as a source can be used also on a Unix host since, despite not mandatory in that casae, can speed up and standardize the steps across different trials.

## The control of the arm is unintuitive

Yes, it is. The solution proposed to track the user's motion is, definitely not the best one possible. Some (quite a few) updates have been made to the one proposed in the source repository. However, the whole design needs to be remade from the ground up, as a more intuitive control might tracking the user's joints configuration rather than their screen-space position; aka, "my fingers are open/close" rather than "my fingers are up there, no matter if the hand is open or closed".
But, unfortunately, that would require probably quite some time to be implemented in a robust enough way, and might not provide significantly better results for some of the human joints (the shoulders and the nose).
