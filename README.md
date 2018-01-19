# DeepSol: A Deep Learning Framework for Sequence-Based Protein Solubility Prediction

![alt text](http://people.csail.mit.edu/sameerk/dsol.svg)

# Setting up the environment:

This step will install all the dependencies required for running DeepSol in an Anaconda virtual environment locally. You do not need sudo permissions for this step.

## Install Anaconda
1. Download Anaconda installer python3.* for linux : https://www.anaconda.com/download/#linux
2. Run the installer : `bash Anaconda3-5.0.1-Linux-x86_64.sh` and follow the instructions to install anaconda at your preferred location

## Create the environment
1. Run `export PATH=<your_anaconda_folder>/bin:$PATH`
2. Run `conda env create -f environment.yml`
3. Run `source activate dsol`


# Recipe for running DeepSol:

recipe is contained in the script `run.sh`. To see the options run `./run.sh` and you shall see the following:

```
main options (for others, see top of script file)
  --model (deepsol1/deepsol2|deepsol3)               # model architecture to use
  --mode  (train/decode)                             # train up a new model or use an existing model
  --stage (1/2)                                      # point to run the script from 
  --conf_file                                        # model parameter file
  --keras_backend                                    # backend for keras
  --cuda_root                                        # the path cuda installation
  --device (cuda/cpu)                                # device to use for running the recipe
```
There are two stages in the script. 

1. Data preparation
2. Model building and decoding, `--mode train`, or decoding using existing model `--mode decode`.

the recipe supports gpu usage using the option `--device cuda`, if you may wish. More details in the GPU section. Rest of the options are self explanatory and can be modified as desired.

## Run on CPU

Use the following command:

`./run.sh --model deepsol1 --stage 1 --mode train --device cpu data/protein.data`

Note that we used `--model deepsol1`. you can use `deepsol2` or `deepsol3`

In case you want to use the existing models that we have provided in the folder `results/models` to decode the data. You can run the following command:

`./run.sh --model deepsol1 --stage 2 --mode decode --device cpu data/protein.data`

Note that we use `--stage 2` because we do not want to perform Data Preparation in this case. We already have the data that we need to decode using `deepsol1`, provided at `results/models/deepsol1`

## GPU

### Cuda installation

First ensure that you have cuda installed. We support Cuda 8.0 and Cudnn 5.1 . If you use any other version of Cudnn, you misht run into some issues.

Install Cuda 8.0 and Cudnn 5.1 from https://developer.nvidia.com/

We tested our code against GeForce GTX 1080 Nvidia GPUs https://www.nvidia.com/en-us/geforce/products/10series/geforce-gtx-1080/ . The GPU driver version is 384.59

We also tested on Nvidia Tesla K20Xm : https://www.techpowerup.com/gpudb/1884/tesla-k20xm with driver version 375.66

### Run on GPU

Use the following command:

`./run.sh --model deepsol1 --stage 1 --mode train --cuda_root <path-to-your-cuda-installation> --device cuda data/protein.data`

Note that we used `--model deepsol1`. you can use `deepsol2` or `deepsol3`. Also, `--cuda_root` should you the path to your cuda installation. By default it is `/usr/local/cuda`

In case you want to use the existing models that we have provided in the folder `results/models` to decode the data. You can run the following command:

`./run.sh --model deepsol1 --stage 2 --mode decode --cuda_root <path-to-your-cuda-installation> --device cuda data/protein.data`

Note that we use `--stage 2` because we do not want to perform Data Preparation in this case. We already have the data that we need to decode using `deepsol1`, provided at `results/models/deepsol1`
