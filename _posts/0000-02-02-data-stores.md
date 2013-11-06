---
layout: default
title:  "Data stores"
tag: customization
---

# Data stores
Data stores are key-value stores that store a piece of content with meta (the value) and reference it with a string uid (the key).

Dragonfly uses these to store data which can later be fetched, e.g.

Given any Dragonfly job
{% highlight ruby %}
job = Dragonfly.app.generate(:text, "lublug").thumb('x200')
{% endhighlight %}

it can be stored with
{% highlight ruby %}
uid = job.store   # ===> "2013/11/06/12_38_39_606_taj.jpg"
{% endhighlight %}

and later fetched with
{% highlight ruby %}
Dragonfly.fetch(uid)
{% endhighlight %}

### Models
Models simply hold a reference to the uid and do the storing and fetching behind the scenes at the appropriate times - see [Models]({% post_url 0000-01-04-models %}) for more details.

## File data store
This is the default, but it can be manually configured using
{% highlight ruby %}
Dragonfly.app.configure do
  datastore :file
  # ...
end
{% endhighlight %}

or with options
{% highlight ruby %}
datastore :file,
  :root_path => 'public/dragonfly',    # directory under which to store files
                                       # - defaults to 'dragonfly' relative to current dir
  :server_root => 'public'             # root for urls when serving directly from datastore
                                       #   using remote_url
{% endhighlight %}

You can specify the storage path per-content with
{% highlight ruby %}
uid = job.store(:path => 'my/custom/path')
{% endhighlight %}

To see how to do this with models, see [Models - Storage Options]({% post_url 0000-01-04-models %}#storage-options)

## Memory data store
The Memory data store keeps everything in memory and is useful for things like tests.

To use:
{% highlight ruby %}
Dragonfly.app.configure do
  datastore :memory
  # ...
end
{% endhighlight %}

You can also specify the uid on store
{% highlight ruby %}
uid = job.store(:uid => "179")
{% endhighlight %}

## Other data stores
The following datastores previously in Dragonfly core are now in separate gems:

  - [S3](https://github.com/markevans/dragonfly-s3_data_store)
  - [Couch](https://github.com/markevans/dragonfly-couch_data_store)
  - [Mongo](https://github.com/markevans/dragonfly-mongo_data_store)

## Building a custom data store

