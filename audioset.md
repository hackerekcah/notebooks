# [Audioset](https://research.google.com/audioset/index.html)
* [Data split](https://research.google.com/audioset/download.html)
* 2w(55.55hours) balanced train and 2w balanced eval

## [ESC-50](https://github.com/karolpiczak/ESC-50)
## [l3embedding](https://github.com/marl/l3embedding)
* tools to train l3net

## [audiosetdl](https://github.com/marl/audiosetdl)
* tools to download audioset

# [fairseq](https://github.com/pytorch/fairseq)
* sequence modeling based on pytorch
* [documentation](https://fairseq.readthedocs.io/en/latest/)
* [command-line](https://fairseq.readthedocs.io/en/latest/command_line_tools.html)
* env: torch151
## wav2vec
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
