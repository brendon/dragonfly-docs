---
layout: default
title:  "Processors"
tag: customization
---

# Processors
A Processor modifies content, e.g. resizing an image, converting audio to mp3 format, etc.

They can be added using a block
{% highlight ruby %}
Dragonfly.app.configure do
  processor :shrink do |content|
    # ...
  end
  # ...
end
{% endhighlight %}

or providing an object that responds to `call` (`MyProcessor` in this case)
{% highlight ruby %}
Dragonfly.app.configure do
  processor :shrink, MyProcessor
  # ...
end
{% endhighlight %}

## Using the processor
The processor is available as a method to `Job` objects

{% highlight ruby %}
image = Dragonfly.app.fetch('some/uid')
smaller_image = image.shrink
{% endhighlight %}

and `Attachment` objects

{% highlight ruby %}
image = my_model.photo
smaller_image = image.shrink
{% endhighlight %}

Furthermore, a bang! method is provided, which operates on `self`
{% highlight ruby %}
image.shrink!
{% endhighlight %}

## Implementing the processor
The `content` object yielded to the block/`call` method is a <a href="http://rdoc.info/github/markevans/dragonfly/Dragonfly/Content" target="_blank">Dragonfly::Content - see the doc</a> for methods it provides.

The processor's job is to use methods on `content` to modify it - the return value of the processor block is not important.

### Updating content and metadata
The primary method to use to update content is `Content#update`. It can take a String, Pathname, File or Tempfile, and optionally metadata to add.

{% highlight ruby %}
processor :shrink do |content|
  # ...
  content.update(some_file, 'some' => 'meta')
end
{% endhighlight %}

Another way of updating metadata is with `add_meta`
{% highlight ruby %}
content.add_meta('some' => 'meta')
{% endhighlight %}

**NOTE** meta data should be serializable to and from JSON.

### Using shell commands
To update using the shell, you can use `Content#shell_update`

{% highlight ruby %}
processor :shrink do |content|
  content.shell_update do |old_path, new_path|
    "/usr/bin/shrink #{old_path} -o #{new_path}"  # The command sent to the command line
  end
end
{% endhighlight %}

The yielded `old_path` and `new_path` above will always exist.

### Using pre-registered processors
To update using a pre-registered processor, use `Content#process!`

{% highlight ruby %}
processor :greyscale do |content|
  content.process! :convert, "-type Grayscale"
end
{% endhighlight %}

## ImageMagick
The ImageMagick plugin adds a few processors - see [the doc]({% post_url 0000-01-05-imagemagick %}) for more details.
