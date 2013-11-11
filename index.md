---
layout: default
title: Dragonfly
---

TODO

<p>
  Created and maintained by <a href="https://github.com/markevans">Mark Evans</a><br />
  <a href="https://github.com/markevans/dragonfly/graphs/contributors">Contributors</a><br />
  <small>Derived from theme by <a href="https://github.com/orderedlist">orderedlist</a></small>
</p>
<p>
  <a href="https://github.com/markevans/dragonfly">GitHub</a> <br />
  <a href="http://rubydoc.info/github/markevans/dragonfly/frames">RubyDoc</a> <br />
  <a href="http://github.com/markevans/dragonfly/issues">Issues</a> <br />
  <a href="http://groups.google.com/group/dragonfly-users">Suggestions/Questions</a> <br />
  {% for post in site.posts %}
  {% if post.tags contains 'changelog' %}
  <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a> <br />
  {% endif %}
  {% endfor %}
  <a href="{{ site.baseurl }}/v0.9.15">Docs for version 0.9.x</a>
</p>
