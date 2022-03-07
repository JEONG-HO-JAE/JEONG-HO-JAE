---
title: "Plogram Language"
layout: archive
permalink: categories/pl
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.pl %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
