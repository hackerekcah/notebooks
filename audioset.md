# [Audioset](https://research.google.com/audioset/index.html)
* [Data split](https://research.google.com/audioset/download.html)
* 2w(55.55hours) balanced train and 2w balanced eval

## [ESC-50](https://github.com/karolpiczak/ESC-50)
## [GTZAN](http://marsyas.info/downloads/datasets.html)
* Music gengre classification
## [Urbansound8K](https://urbansounddataset.weebly.com/urbansound8k.html)
* 8732 excerpts
```
* classID:
A numeric identifier of the sound class:
0 = air_conditioner
1 = car_horn
2 = children_playing
3 = dog_bark
4 = drilling
5 = engine_idling
6 = gun_shot
7 = jackhammer
8 = siren
9 = street_music
```
## [l3embedding](https://github.com/marl/l3embedding)
* tools to train l3net

## [audiosetdl](https://github.com/marl/audiosetdl)
* tools to download audioset

# [fairseq](https://github.com/pytorch/fairseq)
* sequence modeling based on pytorch
* [documentation](https://fairseq.readthedocs.io/en/latest/)
* [command-line](https://fairseq.readthedocs.io/en/latest/command_line_tools.html)
* [wav2vec example](https://github.com/pytorch/fairseq/tree/master/examples/wav2vec)
* [wav2vec model](https://github.com/pytorch/fairseq/blob/master/fairseq/models/wav2vec.py)
* env: torch151
## wav2vec
* Load checkpointer, validate on my dataset, dont train.
  * command
```
python /data/songhongwei/proj/fairseq/fairseq_cli/validate.py \
/data/songhongwei/audioset/env2vec/manifest/ \                        # manifest directory, containing train.tsv, valid.tsv, valid2.tsv...
--path ... \                                                          # checkpoint path
--valid-subset valid,valid2 \                                         #comma seperated
--max-sentences
--tensorboard-logdir /data/songhongwei/audioset/env2vec/${EXP_NAME}/log/ \
--num-negatives 10 \
--num-workers 1 --save-interval 1 \
--arch wav2vec --task audio_pretraining --skip-connections-agg --residual-scale 0.5 --log-compression \
--criterion binary_cross_entropy \
--max-sample-size 160000 \
--max-sentences 40 \
--required-batch-size-multiple 1 \
--skip-invalid-size-inputs-valid-test \
--distributed-no-spawn \
--conv-feature-layers [(512, 10, 5), (512, 8, 4), (512, 4, 2), (512, 4, 2), (512, 4, 2), (512, 1, 1), (512, 1, 1)] \
--conv-aggregator-layers [(512, 2, 1), (512, 3, 1), (512, 4, 1), (512, 5, 1), (512, 6, 1), (512, 7, 1), (512, 8, 1), (512, 9, 1), (512, 10, 1), (512, 11, 1), (512, 12, 1), (512, 13, 1)] \
```
  * parameters
```
--restore-file [ckpt_file]
--reset-dataloader
```
* Load checkpointer, finetune on my dataset
```
--restore-file
--reset-dataloader
--reset-lr-scheduler
--reset-optimizer
using small learning rate to finetune
```

* Parameters
```
"Must specify batch size either with --max-tokens or --max-sentences"
```
* Logging
```
train.py
  @metrics.aggregate("train")
  def train(args, trainer, task, epoch_itr):
      # log end-of-epoch stats
    stats = get_training_stats(metrics.get_smoothed_values("train"))
    progress.print(stats, tag="train", step=num_updates)

2020-06-28 19:15:09 | INFO | train | epoch 066 | loss 2.68652 | accuracy 0.249573 | wps 378315 | ups 0.42 | wpb 895107 | bsz 895107 | num_updates 858 | lr 0.0049446 | gnorm 0.79 | clip 0 | train_wall 30 | wall 2038
```
```
# Hint: Use ctrl+shift+F to search over all the project, for finding the logging code in the project
train_wall: seconds used to train one setp (i.e., one update)
ups: update per seconds (averaged)
wall: total seconds used.
wpb: word(i.e., tokens) per batch
wps: word per second
bsz: sentences per second, batch_size.(in wav2vec, total "examples" for binary classification)
```

* `fairseq/data/audio/raw_audio_dataset.py`
  * default resample to 16K hz
  * mean if have two channel
```
class FileAudioDataset(RawAudioDataset):
  self.fnames: list of files
  # todo: a bug when resample
  self.sizes: list of file sample size
  
RawAudioDataset(FairseqDataset):
  """
  1:for samples in a batch, sample is randomly cliped to the minimum len of the samples
  2: return ordered indecies for batching, by audio len, and by a shuffle (order by audio len first, same audio len, sort by random shuffled index)
  """
```
* 
```
        epoch_itr = trainer.get_train_iterator(
            epoch=1, load_dataset=True, **passthrough_args
        ):
           self.task.load_dataset( 
                self.args.train_subset,
                epoch=epoch,
                combine=combine,
                data_selector=data_selector,)
                
           return self.task.get_batch_iterator(
                dataset=self.task.dataset(self.args.train_subset),
                max_tokens=self.args.max_tokens,
                max_sentences=self.args.max_sentences,
                max_positions=utils.resolve_max_positions(
                    self.task.max_positions(),
                    self.model.max_positions(),
                    self.args.max_tokens,
                ),
                ignore_invalid_inputs=True,
                required_batch_size_multiple=self.args.required_batch_size_multiple,
                seed=self.args.seed,
                num_shards=self.data_parallel_world_size if shard_batch_itr else 1,
                shard_id=self.data_parallel_rank if shard_batch_itr else 0,
                num_workers=self.args.num_workers,
                epoch=epoch
            )
```
* `fairseq/tasks/audio_pretraining.py`
```
@register_task('audio_pretraining')
class AudioPretrainingTask(FairseqTask):

class FairseqTask(object):

    def get_batch_iterator(
        self,
        dataset,
        max_tokens=None,
        max_sentences=None,
        max_positions=None,
        ignore_invalid_inputs=False,
        required_batch_size_multiple=1,
        seed=1,
        num_shards=1,
        shard_id=0,
        num_workers=0,
        epoch=1
    ):
        # create mini-batches with given size constraints
        batch_sampler = data_utils.batch_by_size(
            indices,
            dataset.num_tokens,
            max_tokens=max_tokens,
            max_sentences=max_sentences,
            required_batch_size_multiple=required_batch_size_multiple,
        )

        # return a reusable, sharded iterator
        epoch_iter = iterators.EpochBatchIterator(
            dataset=dataset,
            collate_fn=dataset.collater,
            batch_sampler=batch_sampler,
            seed=seed,
            num_shards=num_shards,
            shard_id=shard_id,
            num_workers=num_workers,
            epoch=epoch,
            buffer_size=getattr(self.args, 'data_buffer_size', 0)
        )
```
