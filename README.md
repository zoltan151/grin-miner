[![Build Status](https://dev.azure.com/mimblewimble/grin-miner/_apis/build/status/mimblewimble.grin-miner?branchName=master)](https://dev.azure.com/mimblewimble/grin-miner/_build/latest?definitionId=5&branchName=master)

# Grin Miner

A standalone mining implementation intended for mining Grin against a running Grin node.

## Supported Platforms

At present, only mining plugins for linux-x86_64 and MacOS exist. This will likely change over time as the community creates more solvers for different platforms.

## Requirements

- rust 1.30+ (use [rustup]((https://www.rustup.rs/))- i.e. `curl https://sh.rustup.rs -sSf | sh; source $HOME/.cargo/env`)
- cmake 3.2+ (for [Cuckoo mining plugins]((https://github.com/mimblewimble/cuckoo-miner)))
- ncurses and libs (ncurses, ncursesw5)
- zlib libs (zlib1g-dev or zlib-devel)
- linux-headers (reported needed on Alpine linux)

And a [running Grin node](https://github.com/mimblewimble/grin/blob/master/doc/build.md) to mine into!

## Build steps


## Build steps

## Step 1:

```sh

## Go to root directory ##
cd /

## Apt update ##
sudo apt-get -y update

## Install the linux headers ##
sudo apt-get -y install linux-headers-$(uname -r)

## Install ncurses and libs ##
sudo apt-get -y install libncurses5-dev libncursesw5-dev

## Install zlib libs ##
sudo apt-get -y install zlib1g-dev

## Apt update and install OpenCL libraries ##
sudo apt-get -y install ocl-icd-opencl-dev

## Install OpenCL drivers ##
sudo apt-get -y install alsa-utils
sudo apt-get -y install intel-opencl-icd
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo ubuntu-drivers autoinstall

## Ensure $PATH env variable is correctly set ##
sudo export PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"

## Delete existing grin-miner directory, if it exists ##
cd /
sudo rm -rf grin-miner

## Install Rust ##
cd /
sudo curl https://sh.rustup.rs -sSf | sudo sh; source $HOME/.cargo/env 

## Install and switch to Rustc version 1.59  - have to repeat twice because it doesn't take the first time, for some reason. Will have to look into that later. ##
rustup install 1.59
rustup default 1.59
rustup default 1.59

## Install cmake 3.2.2 ##
cd /
sudo apt-get -y remove cmake
sudo apt-get -y install build-essential
sudo wget http://www.cmake.org/files/v3.2/cmake-3.2.2.tar.gz
sudo tar -zxvf cmake-3.2.2.tar.gz
cd cmake-3.2.2
./configure
make
sudo make install

## Install GNU / GCC ##
cd /
sudo apt-get -y update
sudo apt-get -y install -y build-essential
sudo apt-get -y install g++
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-8/gcc-8_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.edge.kernel.org/ubuntu/pool/universe/g/gcc-8/gcc-8-base_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-8/libgcc-8-dev_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-8/cpp-8_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-8/libmpx2_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/main/i/isl/libisl22_0.22.1-1_amd64.deb
sudo apt-get -y install ./libisl22_0.22.1-1_amd64.deb ./libmpx2_8.4.0-3ubuntu2_amd64.deb ./cpp-8_8.4.0-3ubuntu2_amd64.deb ./libgcc-8-dev_8.4.0-3ubuntu2_amd64.deb ./gcc-8-base_8.4.0-3ubuntu2_amd64.deb ./gcc-8_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-8/libstdc++-8-dev_8.4.0-3ubuntu2_amd64.deb
wget http://mirrors.kernel.org/ubuntu/pool/universe/g/gcc-8/g++-8_8.4.0-3ubuntu2_amd64.deb
sudo apt-get -y install ./libstdc++-8-dev_8.4.0-3ubuntu2_amd64.deb ./g++-8_8.4.0-3ubuntu2_amd64.deb
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8

## Install CUDA toolkit and Nvidia drivers ##
cd /
#sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
#sudo bash -c 'echo "deb [allow-insecure=yes] http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
#sudo bash -c 'echo "deb [allow-insecure=yes] http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
#sudo apt-get -y update
#sudo apt-get -y install cuda-10-1
#sudo apt-get -y install libcudnn7
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
sudo sh cuda_10.2.89_440.33.01_linux.run
export PATH=/usr/local/cuda-10.2/bin:/usr/local/cuda-10.2/NsightCompute-2019.1${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64\${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
sudo add-apt-repository ppa:graphics-drivers
sudo apt-get -y install ubuntu-drivers-common
sudo apt-get -y install nvidia-driver-535
sudo apt-get -y install nvidia-utils-535
#sudo ubuntu-drivers devices

## Install OpenSSL 1.1.1 and set as default ##
cd /
sudo apt-get -y remove openssl
git clone --depth 1 --branch OpenSSL_1_1_1g https://github.com/openssl/openssl.git
cd openssl
./config zlib '-Wl,-rpath,$(LIBRPATH)'
make
sudo make install
sudo ldconfig -v
source ~/.bashrc
exec bash

```


## Step 2

```sh

## Install libssl-dev ##
sudo apt-get -y update
sudo apt-get -y install libssl-dev

## Install pkg-config ##
sudo apt-get -y update
sudo apt-get -y install pkg-config

## Clone and Build ##
cd /
git clone https://github.com/mimblewimble/grin-miner.git
cd grin-miner
git submodule update --init
cargo build --features opencl 
./install_ocl_plugins.sh
cp grin-miner.toml target/debug/grin-miner.toml
cd target/debug
```


### Building the Cuckoo-Miner plugins

Grin-miner automatically builds x86_64 CPU plugins. Cuda plugins are also provided, but are
not enabled by default. To enable them, modify `Cargo.toml` as follows:

```
change:
cuckoo_miner = { path = "./cuckoo-miner" }
to:
cuckoo_miner = { path = "./cuckoo-miner", features = ["build-cuda-plugins"]}
```

The Cuda toolkit 9+ must be installed on your system (check with `nvcc --version`)

### Building the OpenCL plugins
OpenCL plugins are not enabled by default. Run `install_ocl_plugins.sh` script to build and install them.

```
./install_ocl_plugins.sh
```
You must install OpenCL libraries for your operating system before.
If you just need to compile them (for development or testing purposes) build grin-miner the following way:

```
cargo build --features opencl
```

### Build errors

See [Troubleshooting](https://github.com/mimblewimble/docs/wiki/Troubleshooting)

## What was built?

A successful build gets you:

 - `target/debug/grin-miner` - the main grin-miner binary
 - `target/debug/plugins/*` - mining plugins

Make sure you always run grin-miner within a directory that contains a
`grin-miner.toml` configuration file.

While testing, put the grin-miner binary on your path like this:

```
export PATH=/path/to/grin-miner/dir/target/debug:$PATH
```

You can then run `grin-miner` directly.

# Configuration

Grin-miner can be further configured via the `grin-miner.toml` file.
This file contains contains inline documentation on all configuration
options, and should be the first point of reference.

You should always ensure that this file exists in the directory from which you're
running grin-miner.

# Using grin-miner

There is a [Grin forum post](https://www.grin-forum.org/t/how-to-mine-cuckoo-30-in-grin-help-us-test-and-collect-stats/152) with further detail on how to configure grin-miner and mine grin's testnet.
