---
title: "Projects"
layout: page
permalink: /projects.html
---

Here's some example projects!

{% include heading.html heading="Coding Projects" size="large" %}
<p style="text-align: left;">
{% for project in site.codingprojects %}
{{project.publish_date}}: <a href="{{project.url}}">{{project.title}}</a>
<br>
{% endfor %}
</p>

{% include heading.html heading="Cooking Projects" size="large" %}
<p style="text-align: left;">
{% for project in site.cookingprojects %}
<a href="{{project.url}}">{{project.title}}</a>
<br>
{% endfor %}
</p>

