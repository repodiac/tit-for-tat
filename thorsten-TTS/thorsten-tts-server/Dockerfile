ARG BASEDIR=/tts
# use existing Mozilla TTS installation
FROM thorsten-tts-training as ttts_base
COPY --from=thorsten-tts-training /tts/TTS $BASEDIR/TTS

WORKDIR $BASEDIR/TTS/mozilla_voice_tts/server
# if you would like to use a GPU, set "--use_cuda" to "True"
ENTRYPOINT ["python", "server.py", "--tts_config", "/tts/model_config.json", "--use_cuda", "False", "--tts_checkpoint"]
CMD [""]
