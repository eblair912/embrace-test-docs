---
layout: page
title: Changelog
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
