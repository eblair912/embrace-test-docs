---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Home
nav_order: 1
---

<h1>{{ page.title }}</h1>

<ul>
    {% for post in site.posts %}
        <li>
            <a href="{{ post.url }}">{{ post.title }}</a>
            <p>{{ post.excerpt }}</p>
            <small>Posted on {{ post.date | date: "%B %d, %Y" }}</small>
        </li>
    {% endfor %}
</ul>
