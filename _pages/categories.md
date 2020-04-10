---
layout: post
title: "Categories"
author: "TY_K"
permalink: /categories/
---

<div>
    <h2> network </h2>
    {% for network in site.categories.network %}
        <div>
            - <a href="{{ network.url | prepend: site.baseurl }}">{{ network.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> web </h2>
    {% for web in site.categories.web %}
        <div>
            - <a href="{{ web.url | prepend: site.baseurl }}">{{ web.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> OS </h2>
    {% for os in site.categories.os %}
        <div>
            - <a href="{{ os.url | prepend: site.baseurl }}">{{ os.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> JS </h2>
    {% for JS in site.categories.JS %}
        <div>
            - <a href="{{ JS.url | prepend: site.baseurl }}">{{ JS.title }}</a>
        </div>
    {% endfor %}
</div>
