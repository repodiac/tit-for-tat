# Docker "Turn-Key Solutions" for Training with @thorstenMueller's German TTS Dataset

Multiple settings via specific Dockerfile each, for use in training with [@thorstenMueller](https://github.com/thorstenMueller)'s [German TTS dataset](https://github.com/thorstenMueller/deep-learning-german-tts).

See subfolders for each setting/config and corresponding file(s):

**Training**

* `thorsten-v02-DDC`: Tacotron2+DDC with ParallelWaveGAN as vocoder component; config/settings by [@erogol](https://github.com/erogol/TTS_recipes/tree/master/Thorsten_DE/DoubleDecoderConsistency)
* `thorsten-v02-DDC_espeak-ng_loan_words`: same as above but makes use of a German loan words dictionary for espeak-ng, improving phonemic encodings of specifically loan words ;-)
* `thorsten-v02-DDC_chars_only`: despite the former two configs, this setting only uses graphemes or simply the text from the training data -- **no phonemes** or the like

**Inference**

* `thorsten-tts-server`: When a model has been trained via one of the configurations above, you can easily setup a demo/dev server for testing with this Dockerfile
