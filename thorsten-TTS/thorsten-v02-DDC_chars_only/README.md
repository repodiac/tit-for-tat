**IMPORTANT NOTE:**
* This is a specific version using **no phonemes**, so only graphemes or only characters in the training text is used!
* It also makes use of [Apex](https://github.com/NVIDIA/apex), an extension from NVIDIA for automatic mixed-precision training - significantly saving memory and (often) also speeding up calculations!

# Installation (under Linux)

1. Clone this repo
```
git clone https://github.com/repodiac/tit-for-tat thorsten-tts-training
```

2. Change to the cloned folder structure
```
cd thorsten-tts-training/thorsten-TTS/thorsten-v02-DDC_chars_only  
```

3. Make sure the config files `model_config.json` and `vocoder_config.json` are in the same folder as the `Dockerfile`
```
docker build --rm -t thorsten-tts-training .
```

# Usage

Consider **increasing the shared memory size**: Per default, only 64MB of shared memory are allocated by docker - this can cause issues with preprocessing and DataLoaders; 256 MB seems to work just fine

```
docker run --rm --shm-size=256m -v $PWD/models:/tts/models thorsten-tts-training
```

If you run it with **NVIDIA GPU(s)**, please install the nvidia-docker extension beforehand (https://github.com/NVIDIA/nvidia-docker), then add the CLI parameter `--gpus all` to enable the GPU(s) in the container

```
docker run --rm --shm-size=256m --gpus all -v $PWD/models:/tts/models thorsten-tts-training
```

## Continue training after break / restore from checkpoints

When you have stopped training sometime (note: you can stop the container anytime via `docker stop $(docker container ls | grep 'thorsten-tts-training' | cut -f1 -d\t)`) you can easily resume/restore from the last checkpoint or best model trained so far (note: Mozilla TTS chooses the model with the most steps, automatically):

```
docker run --rm --shm-size=256m --gpus all -v $PWD/models:/tts/models thorsten-tts-training --continue_path /tts/models/<here specific path to model file within models subfolder (see Note)>
```

**Note:** Finished models/checkpoints are stored in subfolder `models` after the training!

