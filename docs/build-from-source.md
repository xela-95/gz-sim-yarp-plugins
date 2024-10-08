# Build gz-sim-yarp-plugins from source

## Table of Contents

- [Dependencies](#dependencies)
- [Compile from source using conda-forge dependencies on Linux, macOS or Windows](#compile-from-source-using-conda-forge-dependencies-on-linux-macos-or-windows)
- [Compile from source using apt dependencies on Linux, macOS or Windows](#compile-from-source-using-apt-dependencies-on-linux-macos-or-windows)
- [Usage](#usage)

## Dependencies

`gz-sim-yarp-plugins` is a fairly classical C++ project build with CMake, so it should be quite easy to build if you are already familiar with how you build C++ projects with CMake.
If you are not familiar with the use of CMake, you can check some documentation on <https://cmake.org/runningcmake/> or <https://cgold.readthedocs.io>.

Before building `gz-sim-yarp-plugins`, you need to install its dependencies, the main ones being:

- [Modern Gazebo](https://gazebosim.org/home)
- [YARP](https://yarp.it/latest//)
- [YCM CMake Modules](https://robotology.github.io/ycm/gh-pages/latest/index.html#)

in addition to the usual dependencies used to configure, compile and test C++ packages.

## Compile from source using conda-forge dependencies on Linux, macOS or Windows

If you are using conda (or mamba), the dependencies of `gz-sim-yarp-plugins` can be installed with:

```bash
conda install -c conda-forge libgz-sim8 yarp ycm-cmake-modules cmake ninja pkg-config cmake compilers gtest cli11
```

This command should be executed in a terminal with the environment activated.

### Build

You can then compile abd install `gz-sim-yarp-plugins` itself, using the following commands on **Linux** and **macOS**:

```bash
git clone https://github.dev/robotology/gz-sim-yarp-plugins
mkdir build
cd build
cmake -GNinja -DCMAKE_INSTALL_PREFIX=$CONDA_PREFIX -DCMAKE_PREFIX_PATH=$CONDA_PREFIX ..
ninja
ninja install
```

or the following commands on **Windows**:

```bash
git clone https://github.dev/robotology/gz-sim-yarp-plugins
mkdir build
cd build
cmake -GNinja -DCMAKE_INSTALL_PREFIX=%CONDA_PREFIX%\Library -DCMAKE_PREFIX_PATH=%CONDA_PREFIX%\Library ..
ninja
ninja install
```

## Compile from source using apt dependencies on Linux, macOS or Windows

If you are using an apt-based distribution such as Ubuntu and you want to use apt, the dependencies can be installed via:

```bash
sudo apt-get update
sudo apt-get install lsb-release wget gnupg cmake pkg-config ninja-build build-essential libcli11-dev libgtest-dev
```

and then install Gazebo Harmonic:

```bash
sudo wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt-get update
sudo apt-get install gz-harmonic
```

Then, you need to install [`ycm-cmake-modules`](https://github.com/robotology/ycm) and [`yarp`](https://github.com/robotology/yarp), for which no apt binaries are available. You can install them easily via the `robotology-superbuild`, or otherwise with the following commands:

```bash
mkdir ~/gsyp_ws
cd ~/gsyp_ws
git clone https://github.com/robotology/ycm
git clone https://github.com/robotology/yarp
cd ycm
mkdir build
cd build
cmake -GNinja -DCMAKE_INSTALL_PREFIX=~/gsyp_ws/install -DCMAKE_PREFIX_PATH=~/gsyp_ws/install ..
ninja
ninja install
cd ~/gsyp_ws/yarp
cd yarp
git checkout v3.9.0
mkdir build
cd build
cmake -GNinja -DCMAKE_INSTALL_PREFIX=~/gsyp_ws/install -DCMAKE_PREFIX_PATH=~/gsyp_ws/install ..
ninja
ninja install
```

Then, build and install `gz-sim-yarp-plugins` itself:

```bash
git clone https://github.dev/robotology/gz-sim-yarp-plugins
mkdir build
cd build
cmake -GNinja -DCMAKE_INSTALL_PREFIX=~/gsyp_ws/install -DCMAKE_PREFIX_PATH=~/gsyp_ws/install ..
ninja
ninja install
```

## Usage

To notify Gazebo of the new plugins compiled, it is necessary to modify the `GZ_SIM_SYSTEM_PLUGIN_PATH` environment variable, for example on Linux:

~~~
export GZ_SIM_SYSTEM_PLUGIN_PATH=${GZ_SIM_SYSTEM_PLUGIN_PATH}:<install_location>/lib
~~~

where `<install_location>` is the directory passed to `CMAKE_INSTALL_PREFIX` during the CMake configuration.

To ensure that the tools are found, instead you need to add their location to the `PATH`:

~~~
export PATH=${PATH}:<install_location>/bin
~~~

