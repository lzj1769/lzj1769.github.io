---
title: "News"
layout: textlay
sitemap: false
permalink: /allnews.html
---

# News

### 2023

* **January 13** Our paper scMEGA is publised in [Bioinformatics Advances](https://academic.oup.com/bioinformaticsadvances/advance-article/doi/10.1093/bioadv/vbad003/6986159). Check the tutorial [here](https://costalab.github.io/scMEGA/).

{% for article in site.data.news %}
<p><b> {{ article.date }} </b> - {{ article.headline}}</p>
{% endfor %}
