---
layout: page
title: Memo
---
<div class="category">
    <ul>
    {% for post in site.categories.memo %}
        <li>
            <h2>
            	<a href="{{ post.url }}">{{ post.title }}</a>
            </h2>
            <span>{{ post.description }}</span>
        </li>
    {% endfor %}
    </ul>
</div><!-- .entry -->
