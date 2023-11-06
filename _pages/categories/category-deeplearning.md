---
title: "Basic"
layout: archive
permalink: categories/Basic1
author_profile: true
sidebar_main: true
---

***

{% assign posts = site.categories.Basic %}
{% for post1 in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
