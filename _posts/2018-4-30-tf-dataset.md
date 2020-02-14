---
layout: post
title: Using Dataset in parallelized Tensorflow - an ultra-brief note
---

I used Tensorflow's feed dictionary for quite a while before I realized that
it was the worst possible way to pass data to your model. Then I switched to
`tf.data.Dataset` which is a neat and decent approach to load data by batch.
In the case of distributed Tensorflow, `Dataset` might be seemingly less
straightforward, yet actually super simple to do with multiple workers. 
A side note here is that I use `Horovod` to implement parallelized Tensorflow,
because TF's native parallelization solution uses TCP/IP for workers' 
intercommunication which is slower compared to MPI, and is also kind of 
painful to code. So with `Horovod` up and running, I can set up a `Dataset`
object in this way that each worker will read a different minibatch of the
whole data, with its order randomly shuffled:

```python
dset = tf.data.Dataset.from_tensor_slices(arr)
dset = dset.shard(hvd.size(), hvd.rank())
dset = dset.shuffle(buffer_size=100)
dset = dset.repeat()
dset = dset.batch(minibatch_size)
```

The role of `dset.shard(size, ind)` is to divide the whole dataset into `size`
parts in an interlaced fashion, and take the `ind`-th part for a specific
worker. Say you have a dataset of `[1, 2, 3, 4, 5, 6]` and two workers. 
You can use `shard` to split the dataset into 2 batches `[1, 3, 5]` and
`[2, 4, 6]`, which are then respectively visible to the 2 workers. Without
`shard`, each of your workers will see and read the entire dataset. 

`dset.shuffle()` serves to randomize the order of the elements. The 
`buffer_size` argument is the number of elements to be "pre-fetched" before
randomization. To achieve full randomness, this value should be equal or 
larger than the size of the whole dataset as long as it fits in your memory.

The last two lines are straightforward. `dset.repeat()` means you want to
reiterate the dataset once you have covered all of its elements. 
`dset.batch()` gives you a minibatch of data each time you call `get_next()`.

The order matters here! To get your `Dataset` working in a distributed script,
`shard` must go first, and `shuffle` must comes before `batch`. 

