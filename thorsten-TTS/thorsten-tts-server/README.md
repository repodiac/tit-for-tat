**IMPORTANT NOTE:** You need to already have built a docker container `thorsten-tts-training` via one of the possible configurations, see [README](https://github.com/repodiac/tit-for-tat/tree/master/thorsten-TTS/README.md) before you can use this docker image!

# Installation (under Linux)

1. Change to the folder `thorsten-tts-server` with this `Dockerfile`

2. Run the build

```
docker build --rm -t thorsten-tts_server .
```

# Usage

1. Change to the folder `thorsten-tts-server` again

2. Make sure you have trained a model already, i.e. a `best_model.pth.tar` or any checkpoint exists; you need to insert the file path to this file in the command below:

3. Run the docker container

```
docker run --rm -v $(pwd)/models:/tts/models -p 127.0.0.1:5002:5002 tts_server /tts/models/<here specific file path to model file, e.g. best_model.pth.tar or specific checkpoint>
```

**Note:** You can stop the container anytime via `docker stop $(docker container ls | grep 'thorsten-tts_server' | cut -f1 -d\t)`
