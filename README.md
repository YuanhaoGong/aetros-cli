# AETROS Worker

This package is a python application you need to use when you want to train your deep artificial neural networks locally.

It basically retrieves all model information from AETROS, compiles and starts the training, attached with a special logger
callback that sends all information to AETROS Trainer so you can monitor the whole training.

It also contains dataset provider (`aetros.auto_dataset`, with downloader, generator, in-memory iterator and augmentor) for image datasets
which is used if you have a image dataset configured in AETROS TRAINER.

More information at: http://aetros.com/trainer

## Installation

```bash
$ pip install aetros
```

Requirement: Python v2.x, Theano

## Installation development version

If you want to install current master (which is recommended during the closed-beta) you need to execute:

```bash
$ git clone https://github.com/aetros/aetros-cli.git
$ cd aetros-cli
$ python setup.py
$ python -m aetros
```

You can alternatively to `git clone` download the zip at https://github.com/aetros/aetros-cli/archive/master.zip.

## Usage

```bash
# show help with all available commands and options
$ aetros start --help
$ aetros predict --help

# create new training
$ API_KEY='MY_API_KEY' aetros start my/my-network --insights
> Training 'x82d0r' created and started. Open http://aetros.com/trainer/app?training=x82d0r to monitor the training.


# re-start a training
$ API_KEY='MY_API_KEY' aetros start x82d04
> Training 'x82d0r' restarted. Open http://aetros.com/trainer/app?training=x82d0r to monitor the training.
```

Use CTRL+C stop gracefully stop the job. Press CTRL+C again to force shutdown.

Please note: If you use image datasets AETROS is downloading all images to current working directory at `./datasets/`.
Also you'll find after you handle with networks two additional folders in the current working directory `./networks` and `./weights`.
All those directories will always be created in the current working directory.

### API KEY

In front of each command you need to provide `API_KEY="mykey"` as environment variable. You find your keys at http://aetros.com/user/settings/api.

### GPU

If you have setup CUDA correctly, you can train on NVIDIA GPUs to improve drastically the training speed.
Theano supports NVIDIA GPUs starting with 600 model.

Installation:

* Install CUDA https://developer.nvidia.com/cuda-downloads (we assume you have 7.5)
* Add cuda to your $PATH
 * Mac OSX: `export PATH=$PATH:/Developer/NVIDIA/CUDA-7.5/bin` (place it into `~/.bash_profile` or `~/.zshrc` if you use zsh - restart terminal after change)
 * Linux: `export PATH=$PATH:/usr/local/cuda-7.5/bin` (place it into `~/.bash_profile` or `~/.zshrc` if you use zsh - restart terminal after change)
* Run aetros with `--gpu`: `API_KEY='MY_API_KEY' aetros start my/my-network --gpu`

### CPU Multithreading

Theano supports multithreading to speed up training, however you need to make sure you have OpenMP installed.

To activate multithreading you can pass `--mp 4` to say it should use 4 threads.
This sets the `THEANO_FLAGS` and `OMP_NUM_THREADS` accordingly and may overwrites your settings in `~/.theanorc`.

```bash
$ API_KEY='MY_API_KEY' aetros start my/my-network --mp 4
```