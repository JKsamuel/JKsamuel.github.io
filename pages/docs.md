---
layout: page
title: Documentation
permalink: /docs/
---

# {{ site.title }}

{{ site.author }} 

PROVE THE PROCESS AS A RESULT AND DONE IS MUCH BETTER THAN PERFECT

<div class="section-index">
    <hr class="panel-line">
    {% for post in site.docs  %}        
    <div class="entry">
    <h5><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h5>
    <p>{{ post.description }}</p>
    </div>{% endfor %}
</div>
