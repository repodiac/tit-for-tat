# Adapted from @thorstenMueller's training scripts (https://github.com/thorstenMueller/TTS_recipes) and @erogol's config/settings (https://github.com/erogol/TTS_recipes/tree/master/Thorsten_DE/DoubleDecoderConsistency)

# Docker file by @repodiac (https://github.com/repodiac/tit-for-tat/tree/master/thorsten-TTS)
# *** Use without warranty and at your own risk! ***

# Installation folder BASEDIR **inside** Docker container (usually, do not change!)

# IMPORTANT NOTE: if it is changed to another folder, you have to manually change it...
#   1. ... to the same folder in the last line of this Dockerfile, see CMD
#   2. ... and in external file model_config.json: all occurrences of the former path have to be changed to the new folder, as well
ARG BASEDIR=/tts

FROM pytorch/pytorch:1.6.0-cuda10.1-cudnn7-devel as ttts_base
ARG BASEDIR
WORKDIR $BASEDIR

# Install system libraries etc.
FROM ttts_base as ttts1
#COPY --from=ttts_base / /
ARG BASEDIR
WORKDIR $BASEDIR

RUN apt-get update && apt-get install -y --no-install-recommends \
		build-essential \
		gcc \
		espeak-ng \
		espeak-ng-data \
		git \
		git-extras
RUN pip install pip --upgrade
RUN pip install gdown soundfile

# Download and extract "thorsten-TTS" dataset
FROM ttts1 as ttts2
#COPY --from=ttts1 / /
ARG BASEDIR
WORKDIR $BASEDIR

RUN cd $BASEDIR
RUN gdown https://drive.google.com/uc?id=1mGWfG0s2V2TEg-AI2m85tze1m4pyeM7b -O thorsten-dataset-v02.tgz
RUN tar -xvzf thorsten-dataset-v02.tgz

# Prepare shuffled training and validate data (90% train, 10% val)
RUN shuf thorsten-de/metadata.csv > thorsten-de/metadata_shuf.csv
RUN head -n 20400 thorsten-de/metadata_shuf.csv > thorsten-de/metadata_train.csv
RUN tail -n 2268 thorsten-de/metadata_shuf.csv > thorsten-de/metadata_val.csv

# Install Mozilla TTS repo and dependencies
FROM ttts2 as ttts3
#COPY --from=ttts2 / /
ARG BASEDIR
WORKDIR $BASEDIR

# install NVIDIA Apex extension
RUN git clone https://github.com/NVIDIA/apex
RUN cd apex
RUN pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" $BASEDIR/apex

# checkout a most recent version and install
WORKDIR $BASEDIR
RUN git clone --single-branch --branch dev https://github.com/mozilla/TTS $BASEDIR/TTS
WORKDIR $BASEDIR/TTS
RUN pip install -r requirements.txt
RUN python setup.py develop

# Add german phoneme cleaner library by @repodiac
FROM ttts3 as ttts4
#COPY --from=ttts3 / /
ARG BASEDIR

WORKDIR $BASEDIR
RUN pip install git+https://github.com/repodiac/german_transliterate

WORKDIR $BASEDIR/TTS/mozilla_voice_tts/tts/utils/text
RUN sed '/import re/a from german_transliterate.core import GermanTransliterate' cleaners.py >> cleaners-new.py
RUN mv cleaners-new.py cleaners.py
RUN echo "\ndef german_phoneme_cleaners(text):" >> cleaners.py
RUN echo "\treturn GermanTransliterate(transliterate_ops={'accent_peculiarity', 'amount_money', 'date', 'timestamp', 'time_of_day', 'ordinal', 'special'}).transliterate(text)" >> cleaners.py

# copy config files
WORKDIR $BASEDIR
COPY model_config.json test_sentences.txt* $BASEDIR/
COPY vocoder_config.json $BASEDIR/

FROM ttts4 as ttts5
#COPY --from=ttts4 / /
ARG BASEDIR

# Compute mean and variance of spectrogram features in advance
WORKDIR $BASEDIR/TTS/mozilla_voice_tts/bin/
RUN python compute_statistics.py --config_path /tts/model_config.json --out_path /tts

# Run tacotron2 training and train vocoder PWGAN afterwards
# change the GPU id if needed
WORKDIR $BASEDIR/TTS/mozilla_voice_tts/bin/
#ENTRYPOINT CUDA_VISIBLE_DEVICES="0" python train_tts.py --config_path /tts/model_config.json; CUDA_VISIBLE_DEVICES="0" python train_vocoder.py --config_path /tts/vocoder_config.json
ENTRYPOINT ["python", "train_tts.py"]
CMD ["--config_path", "/tts/model_config.json"]
