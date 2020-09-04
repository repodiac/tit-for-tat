**IMPORTANT NOTE:** This is a specific version using a German loan words dictionary for espeak-ng for improving phonemic encoding

For **how to create** this dictionary, please refer to my specific [repo](https://github.com/repodiac/espeak-ng_german_loan_words)

# Installation (under Linux)

1. Clone this repo
```
git clone https://github.com/repodiac/tit-for-tat thorsten-tts-training
```

2. Change to the cloned folder structuref
cd thorsten-tts-training/thorsten-TTS/thorsten-v02-DDC_espeak-ng_loan_words/
```

3. 

* Make sure the config files `model_config.json` and `vocoder_config.json` are in the same folder as the `Dockerfile`
* We also need the dictionary files `de_extra` and `de_rules` in the **same** folder (see here in my tutorial how to [create/download](https://github.com/repodiac/espeak-ng_german_loan_words))
	* **Please note:** you should be sure, i.e. have checked before, that the dictionary can be imported into espeak-ng (see notes in the tutorial)! Otherwise the build of the Dockerfile might fail.

```
docker build --rm -t thorsten-tts-training-lw .
```

# Usage

Consider **increasing the shared memory size**: Per default, only 64MB of shared memory are allocated by docker - this can cause issues with preprocessing and DataLoaders; 256 MB seems to work just fine

```
docker run -u $(id -u):$(id -g) --rm --shm-size=256m -v $PWD/models:/tts/models thorsten-tts-training-lw
```

If you run it with **NVIDIA GPU(s)**, please install the nvidia-docker extension beforehand (https://github.com/NVIDIA/nvidia-docker), then add the CLI parameter `--gpus all` to enable the GPU(s) in the container

```
docker run -u $(id -u):$(id -g) --rm --shm-size=256m --gpus all -v $PWD/models:/tts/models thorsten-tts-training-lw
```

**Note:** Finished models/checkpoints are stored in subfolder `models` after the training!

