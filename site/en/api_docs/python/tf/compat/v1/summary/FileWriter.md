page_type: reference
<style>{% include "site-assets/css/style.css" %}</style>

<!-- DO NOT EDIT! Automatically generated file. -->

# tf.compat.v1.summary.FileWriter


<table class="tfo-notebook-buttons tfo-api" align="left">

<td>
  <a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L283-L431">
    <img src="https://www.tensorflow.org/images/GitHub-Mark-32px.png" />
    View source on GitHub
  </a>
</td></table>



## Class `FileWriter`

Writes `Summary` protocol buffers to event files.



<!-- Placeholder for "Used in" -->

The `FileWriter` class provides a mechanism to create an event file in a
given directory and add summaries and events to it. The class updates the
file contents asynchronously. This allows a training program to call methods
to add data to the file directly from the training loop, without slowing down
training.

When constructed with a <a href="../../../../tf/compat/v1/Session"><code>tf.compat.v1.Session</code></a> parameter, a `FileWriter`
instead forms a compatibility layer over new graph-based summaries
(`tf.contrib.summary`) to facilitate the use of new summary writing with
pre-existing code that expects a `FileWriter` instance.

<h2 id="__init__"><code>__init__</code></h2>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L298-L370">View source</a>

``` python
__init__(
    logdir,
    graph=None,
    max_queue=10,
    flush_secs=120,
    graph_def=None,
    filename_suffix=None,
    session=None
)
```

Creates a `FileWriter`, optionally shared within the given session.

Typically, constructing a file writer creates a new event file in `logdir`.
This event file will contain `Event` protocol buffers constructed when you
call one of the following functions: `add_summary()`, `add_session_log()`,
`add_event()`, or `add_graph()`.

If you pass a `Graph` to the constructor it is added to
the event file. (This is equivalent to calling `add_graph()` later).

TensorBoard will pick the graph from the file and display it graphically so
you can interactively explore the graph you built. You will usually pass
the graph from the session in which you launched it:

```python
...create a graph...
# Launch the graph in a session.
sess = tf.compat.v1.Session()
# Create a summary writer, add the 'graph' to the event file.
writer = tf.compat.v1.summary.FileWriter(<some-directory>, sess.graph)
```

The `session` argument to the constructor makes the returned `FileWriter` a
compatibility layer over new graph-based summaries (`tf.contrib.summary`).
Crucially, this means the underlying writer resource and events file will
be shared with any other `FileWriter` using the same `session` and `logdir`,
and with any `tf.contrib.summary.SummaryWriter` in this session using the
the same shared resource name (which by default scoped to the logdir). If
no such resource exists, one will be created using the remaining arguments
to this constructor, but if one already exists those arguments are ignored.
In either case, ops will be added to `session.graph` to control the
underlying file writer resource. See `tf.contrib.summary` for more details.

#### Args:


* <b>`logdir`</b>: A string. Directory where event file will be written.
* <b>`graph`</b>: A `Graph` object, such as `sess.graph`.
* <b>`max_queue`</b>: Integer. Size of the queue for pending events and summaries.
* <b>`flush_secs`</b>: Number. How often, in seconds, to flush the
  pending events and summaries to disk.
* <b>`graph_def`</b>: DEPRECATED: Use the `graph` argument instead.
* <b>`filename_suffix`</b>: A string. Every event file's name is suffixed with
  `suffix`.
* <b>`session`</b>: A <a href="../../../../tf/compat/v1/Session"><code>tf.compat.v1.Session</code></a> object. See details above.


#### Raises:


* <b>`RuntimeError`</b>: If called with eager execution enabled.



#### Eager Compatibility
`FileWriter` is not compatible with eager execution. To write TensorBoard
summaries under eager execution, use `tf.contrib.summary` instead.





## Methods

<h3 id="__enter__"><code>__enter__</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L372-L374">View source</a>

``` python
__enter__()
```

Make usable with "with" statement.


<h3 id="__exit__"><code>__exit__</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L376-L378">View source</a>

``` python
__exit__(
    unused_type,
    unused_value,
    unused_traceback
)
```

Make usable with "with" statement.


<h3 id="add_event"><code>add_event</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L394-L401">View source</a>

``` python
add_event(event)
```

Adds an event to the event file.


#### Args:


* <b>`event`</b>: An `Event` protocol buffer.

<h3 id="add_graph"><code>add_graph</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L163-L214">View source</a>

``` python
add_graph(
    graph,
    global_step=None,
    graph_def=None
)
```

Adds a `Graph` to the event file.

The graph described by the protocol buffer will be displayed by
TensorBoard. Most users pass a graph in the constructor instead.

#### Args:


* <b>`graph`</b>: A `Graph` object, such as `sess.graph`.
* <b>`global_step`</b>: Number. Optional global step counter to record with the
  graph.
* <b>`graph_def`</b>: DEPRECATED. Use the `graph` parameter instead.


#### Raises:


* <b>`ValueError`</b>: If both graph and graph_def are passed to the method.

<h3 id="add_meta_graph"><code>add_meta_graph</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L229-L249">View source</a>

``` python
add_meta_graph(
    meta_graph_def,
    global_step=None
)
```

Adds a `MetaGraphDef` to the event file.

The `MetaGraphDef` allows running the given graph via
`saver.import_meta_graph()`.

#### Args:


* <b>`meta_graph_def`</b>: A `MetaGraphDef` object, often as returned by
  `saver.export_meta_graph()`.
* <b>`global_step`</b>: Number. Optional global step counter to record with the
  graph.


#### Raises:


* <b>`TypeError`</b>: If both `meta_graph_def` is not an instance of `MetaGraphDef`.

<h3 id="add_run_metadata"><code>add_run_metadata</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L251-L273">View source</a>

``` python
add_run_metadata(
    run_metadata,
    tag,
    global_step=None
)
```

Adds a metadata information for a single session.run() call.


#### Args:


* <b>`run_metadata`</b>: A `RunMetadata` protobuf object.
* <b>`tag`</b>: The tag name for this metadata.
* <b>`global_step`</b>: Number. Optional global step counter to record with the
  StepStats.


#### Raises:


* <b>`ValueError`</b>: If the provided tag was already used for this type of event.

<h3 id="add_session_log"><code>add_session_log</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L144-L156">View source</a>

``` python
add_session_log(
    session_log,
    global_step=None
)
```

Adds a `SessionLog` protocol buffer to the event file.

This method wraps the provided session in an `Event` protocol buffer
and adds it to the event file.

#### Args:


* <b>`session_log`</b>: A `SessionLog` protocol buffer.
* <b>`global_step`</b>: Number. Optional global step value to record with the
  summary.

<h3 id="add_summary"><code>add_summary</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L101-L142">View source</a>

``` python
add_summary(
    summary,
    global_step=None
)
```

Adds a `Summary` protocol buffer to the event file.

This method wraps the provided summary in an `Event` protocol buffer
and adds it to the event file.

You can pass the result of evaluating any summary op, using
`tf.Session.run` or
<a href="../../../../tf/Tensor#eval"><code>tf.Tensor.eval</code></a>, to this
function. Alternatively, you can pass a <a href="../../../../tf/compat/v1/Summary"><code>tf.compat.v1.Summary</code></a> protocol
buffer that you populate with your own data. The latter is
commonly done to report evaluation results in event files.

#### Args:


* <b>`summary`</b>: A `Summary` protocol buffer, optionally serialized as a string.
* <b>`global_step`</b>: Number. Optional global step value to record with the
  summary.

<h3 id="close"><code>close</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L414-L420">View source</a>

``` python
close()
```

Flushes the event file to disk and close the file.

Call this method when you do not need the summary writer anymore.

<h3 id="flush"><code>flush</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L403-L412">View source</a>

``` python
flush()
```

Flushes the event file to disk.

Call this method to make sure that all pending events have been written to
disk.

<h3 id="get_logdir"><code>get_logdir</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L380-L382">View source</a>

``` python
get_logdir()
```

Returns the directory where event file will be written.


<h3 id="reopen"><code>reopen</code></h3>

<a target="_blank" href="https://github.com/tensorflow/tensorflow/tree/r2.0/tensorflow/python/summary/writer/writer.py#L422-L431">View source</a>

``` python
reopen()
```

Reopens the EventFileWriter.

Can be called after `close()` to add more events in the same directory.
The events will go into a new events file.

Does nothing if the EventFileWriter was not closed.