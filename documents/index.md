---
layout: page
title: Documents
excerpt: "A List of Documents"
comments: false
---

{% capture doc_theme %}
    {% for theme in site.data.documents %}
    {{ theme.theme }} : {{ theme.content.size }}
    {% unless forloop.last %},{% endunless %}
    {% endfor %}
{% endcapture %}
{% assign theme_list = doc_theme | split:',' | sort %}

<ul class="entry-meta inline-list">
  {% for item in (0..site.data.documents.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ theme_list[item] | split: ':' |  first | strip }}{% endcapture %}
  	<li><a href="#{{ this_word }}" class="tag"><span class="term">{{ this_word }}
    {% capture item_size %}{{ theme_list[item] | split: ':' |  last }}{% endcapture %}
    </span> <span class="count">{{ item_size }}</span></a></li>
  {% endunless %}{% endfor %}
</ul>

<div>
{% for item in (0..site.data.documents.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ theme_list[item] | split: ':' |  first | strip }}{% endcapture %}
	<article>
	<h2 id="{{ this_word }}" class="tag-heading">{{ this_word }}</h2>
	<ul>
        {% for theme in site.data.documents %}
            {% if this_word ==  theme.theme %}
            {% for post in theme.content %}
            <li class="entry-title">
            <a href="{{ post.link }}" target="_blank" title="{{ post.title }}">{{ post.title }}</a>
            </li>
            {% endfor%}
            {% endif %}
        {% endfor %}
	</ul>
	</article>
{% endunless %}{% endfor %}
</div>
