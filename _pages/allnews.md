---
title: "News"
layout: textlay
sitemap: false
permalink: /allnews.html
---

# News

{% for article in site.data.news %}
<p>
<b>{{ article.date }}</b>
{{ article.headline | markdownify}}
</p>
{% endfor %}
