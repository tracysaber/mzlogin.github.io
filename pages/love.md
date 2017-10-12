---
layout: page
title: Links
description: 爱情纪念
keywords: 友情链接
comments: true
menu: 爱情纪念
permalink: /love/
---

> God made relatives. Thank God we can choose our friends.

{% for link in site.data.links %}
* [{{ link.name }}]({{ link.url }})
{% endfor %}
