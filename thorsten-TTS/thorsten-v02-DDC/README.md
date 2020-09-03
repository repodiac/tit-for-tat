# Installation (under Linux)

1. Clone this repo
```
git clone https://github.com/repodiac/tit-for-tat thorsten-tts-training
```

2. Change to the cloned folder structure
```
cd thorsten-tts-training/thorsten-TTS/thorsten-v02-DDC/
```

3. Make sure the config files `model_config.json` and `vocoder_config.json` are in the same folder as the `Dockerfile`
```
docker build --rm -t thorsten-tts-training .
```

# Usage

Consider **increasing the shared memory size**: Per default, only 64MB of shared memory are allocated by docker - this can cause issues with preprocessing and DataLoaders; 256 MB seems to work just fine

```
cd thorsten-tts-training
docker run -u $(id -u):$(id -g) --rm --shm-size=256m -v $PWD/models:/tmp/tts/models/thorsten/ thorsten-tts-training
```

If you run it with **NVIDIA GPU(s)**, please install the nvidia-docker extension beforehand (https://github.com/NVIDIA/nvidia-docker), then add the CLI parameter `--gpus all` to enable the GPU(s) in the container

```
cd thorsten-tts-training
docker run -u $(id -u):$(id -g) --rm --shm-size=256m --gpus all -v $PWD/models:/tts/models thorsten-tts-training
```

**Note:** Finished models/checkpoints are stored in subfolder `models` after the training!

