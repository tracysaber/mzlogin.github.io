---
layout: page
title: 爱情纪念
description: 爱情纪念
keywords: 爱情
comments: false
menu: 爱情纪念
permalink: /love/
---

> God made relatives. Thank God we can choose our friends.

{% for link in site.data.links %}
* [{{ link.name }}]({{ link.url }})
{% endfor %}
