---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---
<div class="blog-index">  
  {% assign post = site.posts.first %}
  {% assign content = post.content %}
  {% include post_detail.html %}
</div>
<div class="recent">
          <h3>Recent Posts</h3>
          <ul>
            {% for post in site.posts limit:3 %}
              <a href="{{ post.url }}">
                  <ul>
                  <h3>{{ post.title }}&nbsp;&nbsp;{{ post.date | date_to_string }}</h3>
                  </ul>
              </a>
            {% endfor %}
          </ul>
</div>
