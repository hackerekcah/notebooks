# [Audioset](https://research.google.com/audioset/index.html)
* [Data split](https://research.google.com/audioset/download.html)
* 2w(55.55hours) balanced train and 2w balanced eval

## [l3embedding](https://github.com/marl/l3embedding)
* tools to train l3net

## [audiosetdl](https://github.com/marl/audiosetdl)
* tools to download audioset

# [fairseq](https://github.com/pytorch/fairseq)
* sequence modeling based on pytorch
* env: torch151
## wav2vec
* `fairseq/data/audio/raw_audio_dataset.py`
  * default resample to 16K hz
```
class FileAudioDataset(RawAudioDataset):
  self.fnames: list of files
  # todo: a bug when resample
  self.sizes: list of file sample size
```
