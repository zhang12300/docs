page_type: reference
<style>{% include "site-assets/css/style.css" %}</style>

<!-- DO NOT EDIT! Automatically generated file. -->

# tf.estimator.CheckpointSaverListener


<table class="tfo-notebook-buttons tfo-api" align="left">

<td>
  <a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/training/basic_session_run_hooks.py#L446-L509">
    <img src="https://www.tensorflow.org/images/GitHub-Mark-32px.png" />
    View source on GitHub
  </a>
</td></table>



## Class `CheckpointSaverListener`

Interface for listeners that take action before or after checkpoint save.



### Aliases:

* Class `tf.compat.v1.estimator.CheckpointSaverListener`
* Class `tf.compat.v1.train.CheckpointSaverListener`
* Class `tf.compat.v2.estimator.CheckpointSaverListener`


<!-- Placeholder for "Used in" -->

`CheckpointSaverListener` triggers only in steps when `CheckpointSaverHook` is
triggered, and provides callbacks at the following points:
 - before using the session
 - before each call to `Saver.save()`
 - after each call to `Saver.save()`
 - at the end of session

To use a listener, implement a class and pass the listener to a
`CheckpointSaverHook`, as in this example:

```python
class ExampleCheckpointSaverListener(CheckpointSaverListener):
  def begin(self):
    # You can add ops to the graph here.
    print('Starting the session.')
    self.your_tensor = ...

  def before_save(self, session, global_step_value):
    print('About to write a checkpoint')

  def after_save(self, session, global_step_value):
    print('Done writing checkpoint.')
    if decided_to_stop_training():
      return True

  def end(self, session, global_step_value):
    print('Done with the session.')

...
listener = ExampleCheckpointSaverListener()
saver_hook = tf.estimator.CheckpointSaverHook(
    checkpoint_dir, listeners=[listener])
with
tf.compat.v1.train.MonitoredTrainingSession(chief_only_hooks=[saver_hook]):
  ...
```

A `CheckpointSaverListener` may simply take some action after every
checkpoint save. It is also possible for the listener to use its own schedule
to act less frequently, e.g. based on global_step_value. In this case,
implementors should implement the `end()` method to handle actions related to
the last checkpoint save. But the listener should not act twice if
`after_save()` already handled this last checkpoint save.

A `CheckpointSaverListener` can request training to be stopped, by returning
True in `after_save`. Please note that, in replicated distributed training
setting, only `chief` should use this behavior. Otherwise each worker will do
their own evaluation, which may be wasteful of resources.

## Methods

<h3 id="after_save"><code>after_save</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/training/basic_session_run_hooks.py#L505-L506">View source</a>

``` python
after_save(
    session,
    global_step_value
)
```




<h3 id="before_save"><code>before_save</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/training/basic_session_run_hooks.py#L502-L503">View source</a>

``` python
before_save(
    session,
    global_step_value
)
```




<h3 id="begin"><code>begin</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/training/basic_session_run_hooks.py#L499-L500">View source</a>

``` python
begin()
```




<h3 id="end"><code>end</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/training/basic_session_run_hooks.py#L508-L509">View source</a>

``` python
end(
    session,
    global_step_value
)
```