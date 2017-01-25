# Setup
Our SpaceFortress Q-learning implementation exists mainly of the following parts
* [OpenAI Gym](https://gym.openai.com/)
* [Simple DQN](https://github.com/tambetm/simple_dqn)
* The SpaceFortress Game

### OpenAI Gym
A toolkit for developing and comparing reinforcement learning algorithms. It supports teaching agents everything from walking to playing games like Pong or Go.
Gym will basically create an environment in which the learning algorithm can learn the game.

### Simple DQN
Deep Q-learning agent for replicating DeepMind's results in paper "Human-level control through deep reinforcement learning". It is designed to be simple, fast and easy to extend. This is the workhorse of our project.

### Space Fortress
An old DOS game, but pretty complex nevertheless. We will train a network to play this game.

# Download
Download the repository. Make sure you execute the command in which you want to install the repository.
```sh
sudo git clone https://github.com/Noswis/SpaceFortress.git 
cd SpaceFortress
```

# Installation
During the installation process, please make sure you are in the SpaceFortress folder.
### Neon
[Neon](https://github.com/NervanaSystems/neon) is Nervana's Python based Deep Learning framework and achieves the fastest performance on modern deep neural networks such as AlexNet, VGG and GoogLeNet. Designed for ease-of-use and extensibility. The Simple DQN learning algorithm was built on neon, so we need to install this.

Install prerequisites:
```sh
sudo apt-get install libhdf5-dev libyaml-dev libopencv-dev pkg-config
sudo apt-get install python python-dev python-pip python-virtualenv
```

Check out and compile the code:

```sh
git clone https://github.com/NervanaSystems/neon.git
cd neon
sudo make sysinstall
```
Clean up the dowloaded folder
```sh
cd ..
sudo rm -rf neon
```
### Gym
Install prerequisites:
```sh
# Use 'sudo apt-get install pip' to get pip if you haven't already
sudo pip install -r gym/requirements.txt
```
Connect python to gym
```sh
# add the gym directory to the python path in bashrc
echo "export PYTHONPATH=$PYTHONPATH:$PWD" >> ~/.bashrc
source ~/.bashrc
```
Note: please make sure the path doesn't have any spaces

Check if it is installed correctly:
```sh
python
import gym
```
This shoul produce a warning like this. Any other warning means there is something wrong.
```sh
[2017-01-17 09:44:38,848] Site environment registry incorrect: Scoreboard did not register all envs: set(['AIM-v0', 'SFC-v0', 'SFS-v0', 'Acrobot-v0', 'SF-v0']) Scoreboard registered non-existent or deprecated envs: set(['Acrobot-v1', 'CartPole-v1'])
```
Exit the python environment using the ' exit() ' command.
### Cairo
Cairo is a 2D graphics library with support for multiple output devices. Currently supported output targets include the X Window System (via both Xlib and XCB), Quartz, Win32, image buffers, PostScript, PDF, and SVG file output. Experimental backends include OpenGL, BeOS, OS/2, and DirectFB. The game uses this engine to render scenes. We can get is very easily with aptitude package manager:

```sh
sudo apt-get install libcairo2-dev
```
If you run into any problems installing, please visit [this page](https://www.cairographics.org/download/)
### Shared Libraries
Go to the Game folder
The learning environment needs shared libraries, which can be built from the sourcecode of the SpaceFortress game. In the folder 'game' are multiple subfolders which are subtasks of the original game.
* SF is the game with most elements included 
* AIM is the stripped version of the game which focusses on the aiming task
* SFC is the stripped version of the game which focusses on the control task

Make sure you have the clang compiler installed
```sh
sudo apt-get install clang
```

The Makefile has multple build options,
```sh
cd Game
sudo make SF
sudo make AIM
sudo make SFC
sudo make all    # makes all versions of the game
sudo make clean  # removes all built files
```

### Other dependencies
A list with dependencies we encountered during installation/runtime ourselves.
```sh
sudo apt-get install python-xlib
sudo pip install pynput
sudo pip install pathlib
sudo pip install opencv-python
sudo apt-get install gtk2.0
sudo apt-get install libgtk2.0-dev
sudo apt-get install libgtk-3-dev
sudo apt-get install pkg-config
```

### Testing the environment
Go to the directory with run.py to test if the environment is working. Enter the command:
```sh
cd space_fortress
```
Then enter the command:
```sh
python run.py
```
If a new window appears and the game is running, everything has been installed correctly.
Go to the ' Usage ' section and skip the next section.

If an error occurs that says:
```sh
"OpenCV Error: Unspecified error (The function is not implemented. Rebuild the library with Windows, GTK+ 2.x or Carbon support. If you are on Ubuntu or Debian, install libgtk2.0-dev and pkg-config, then re-run cmake or configure script) in cvNamedWindow, file /io/opencv/modules/highui/src/window.cpp, line 565" 
```

Then read and follow the next section.

### Fixing OpenCV

To be written.


# Usage

### Start Training
The network can be trained with the shell script train.sh located in the Simple_DQN folder. This script calls
the main python script in src along with parameters which specify where to save the weights and results. For instance, the command below will save the training data in ``runs/SFC/MyFirstTraining``.
```sh
./train.sh SFC-v0 MyFirstTraining
```
Instead of SFC, it is also possible to train AIM, SFS and SF.
### Resume Training
With the resume script, a halted training process can be resumed.
```sh
./resume.sh SFC-v0 MyFirstTraining
```
The learning algorithm uses 'epochs', which can be seen as checkpoints. The epochs are saved as .prm files in the weights folder, in the above example, the path to the weights would be ``runs/SFC/MyFirstTraining/weights``. The script will automatically find the most recent checkpoint and load the data. The results and statistics of the training are stored in .csv files. Each time the training is resumed, a new .csv file is created witch a session ID which makes it easier to identify training sessions.

Note: Please make sure the network trained until one epoch before trying to resume. The script will otherwise not work, because no checkpoint was saved.

