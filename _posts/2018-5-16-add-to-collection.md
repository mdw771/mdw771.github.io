---
layout: post
title: Starting a new graph following a previous one&#58 a case study on better understanding Tensorflow ops and graphs
---

As you see I've been using Tensorflow for some purposes that are kind of off the path -
i.e. using its awesome automatic differentiation functionality to do math for me in
solving some optimization problems, instead of using it for machine learning in a formal 
manner. Recently I'm playing with TF on a 3D image reconstruction problem. In order to
improve the overall speed of convergence, I tried to solve the thing in a multiscale
fashion: first, by downsampling the raw data and my object grid to be reconstructed,
I get a coarsely reconstructed object which can be done fast with lower resolution. 
I then use this coarse object as the initial guess for the next iteration, where the
raw data and object grid is 2x as large as the previous one. The result of this run
is fed to the next run as initial guess, until I reach the original resolution of the
object. To do this, I'll need to start a brand new TF model following each lower-resolution
reconstruction (for some reason I cannot keep the same model and just change the data
passed to `placeholder`s; it's a bit hard to explain in a few sentences so let's just
assume it for now). For this, it is straightforward that one thing I need to do is
putting a `tf.reset_default_graph()` after each coarse pass to clear the current graph.
However, the optimizer I defined in the model is still living in my memory. In my
case I declare the optimizer in this way:
```
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate * hvd.size())
optimizer = hvd.DistributedOptimizer(optimizer)
optimizer = optimizer.minimize(loss, global_step=global_step)
```
(The `hvd.DistributedOptimizer(optimizer)` is just a wrapper used by Horovod, a parallelization
package for TF.) By doing this, TF always props up an error message when it comes to line 4
in the second pass:
```
ValueError: Cannot add op with name DistributedAdamOptimizer as that name is already used.
```
So apparently `tf.reset_default_graph()` does not clear the optimizer - it only clears
the nodes. One thing that must be recognized is that Tensorflow has its own internal set
of variables. These fancy little guys are created with the declaration of variables, but
after that they come relatively indenpendent from Python variables. They can also have a
different name from what they are in the Python codes. Hence when I try to
simply overwrite the `optimizer` variable, Tensorflow is not happy with me creating a new
"internal" variable with name "DistributedAdamOptimizer" which is the same as the one created
in the last pass. 

What I did to fix the issue is to specify a unique name for each pass like this:
```
optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate * hvd.size())
optimizer = hvd.DistributedOptimizer(optimizer, name='distopt_{}'.format(ds_level))
optimizer = optimizer.minimize(loss, global_step=global_step)
```
where `ds_level` means "downsampling level", and is unique for each pass. However,
Tensorflow threw out another error when I reran the codes:
```
ValueError: Fetch argument <tf.Operation 'distopt_2' type=AssignAdd> cannot be interpreted as a Tensor. (Operation name: "distopt_2"
op: "AssignAdd"
input: "global_step"
input: "distopt_2/value"
attr {
  key: "T"
  value {
    type: DT_INT32
  }
}
attr {
  key: "_class"
  value {
    list {
      s: "loc:@global_step"
    }
  }
}
attr {
  key: "use_locking"
  value {
    b: false
  }
}
 is not an element of this graph.)
 ```
I was kind of baffled at this point, having no clue why it treated me so badly for being
such a nice guy. It appears from the error message that there is still something wrong with
how I declared the optimizer, but I couldn't figure it out. After struggling I finally 
noticed the line with `s: "loc:@global_step"`.
`global_step` is a `tf.Variable` that I created to count the total number of steps across
different epochs (which is, actually, not quite necessary). Then I realized that the 
decalaration of this variable is **OUT OF** the multiscale loop. The structure of my entire
script is like this:
```
# import stuff, create metadata etc
global_step = tf.Variable(0, trainable=False, name='global_step')

for ds_level in [4, 2, 1]:
    # build model
    optimizer = ...
    optimizer = optimizer.minimize(loss, global_step=global_step)
    # run optimizer and get results
    tf.reset_default_graph()
```
When I reset the default graph, it clears everything including the `global_step`
variable (although the Python variable is still there in the memory!). When the
optimizer tried to get that variable in its argument, Tensorflow could not find
`global_step` in its internal variable set, thus didn't recognize it as a tensor.
This can be fixed easily by moving `global_step` inside the loop, but since it
is of not much use for me, I simply deleted it. 

Another attempt that I did before I fix the issue is adopted from [this webpage](https://stackoverflow.com/questions/43243527/python-tensorflow-how-to-restart-training-with-optimizer-and-import-meta-graph
). The idea is that you can stash your optimizer into a collection during the first
pass; then for following passes, you don't create the optimizer again, but retrieve it
from the collection. For this I declared a flag called `initializer_flag` which is `False`
for the first pass but will subsequently be switched to `True`. So this part of my codes
now become:
```
if initializer_flag == False:
    optimizer = tf.train.AdamOptimizer(learning_rate=learning_rate * hvd.size())
    optimizer = hvd.DistributedOptimizer(optimizer)
    optimizer = optimizer.minimize(loss, global_step=global_step)
    tf.add_to_collection('optimizer', optimizer)
else:
    optimizer = tf.get_collection('optimizer')
```
This may work if you run a standard machine learning program and want to restart training
after stopping it somethere in the mid. The prerequisite is that you **MUST BE USING EXACTLY
THE SAME MODEL**. In my case, when the optimizer is retrieved from the collection, the model
is changed. This can and did lead to undefined behavior of the optimizer. The lesson learned
here is that when you declare an optimizer, it is implicitly linked to, and thus will work only
for the current default graph. 
