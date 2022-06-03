---
layout: post
title: "Categories"
author: "TY_K"
permalink: /categories/
---

<div>
    <h2> network </h2>
    {% assign network_category = site.categories.network | sort: 'date' %}
    {% for network in network_category %}
        <div>
            - <a href="{{ network.url | prepend: site.baseurl }}">{{ network.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> web </h2>
    {% assign web_category = site.categories.web | sort: 'date' %}
    {% for web in web_category %}
        <div>
            - <a href="{{ web.url | prepend: site.baseurl }}">{{ web.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> OS </h2>
    {% assign os_category = site.categories.os | sort: 'date' %}
    {% for os in os_category %}
        <div>
            - <a href="{{ os.url | prepend: site.baseurl }}">{{ os.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> Linux </h2>
    {% assign Linux_category = site.categories.Linux | sort: 'date' %}
    {% for linux in Linux_category %}
        <div>
            - <a href="{{ linux.url | prepend: site.baseurl }}">{{ linux.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> JS </h2>
    {% assign JS_category = site.categories.JS | sort: 'date' %}
    {% for JS in JS_category %}
        <div>
            - <a href="{{ JS.url | prepend: site.baseurl }}">{{ JS.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> Java </h2>
    {% assign Java_category = site.categories.Java | sort: 'date' %}
    {% for Java in Java_category %}
        <div>
            - <a href="{{ Java.url | prepend: site.baseurl }}">{{ Java.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> DB </h2>
    {% assign DB_category = site.categories.DB | sort: 'date' %}
    {% for DB in DB_category %}
        <div>
            - <a href="{{ DB.url | prepend: site.baseurl }}">{{ DB.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> DevOps </h2>
    {% assign DevOps_category = site.categories.DevOps | sort: 'date' %}
    {% for DO in DevOps_category %}
        <div>
            - <a href="{{ DO.url | prepend: site.baseurl }}">{{ DO.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> DataStructure </h2>
    {% assign DataStructure_category = site.categories.DataStructure | sort: 'date' %}
    {% for DS in DataStructure_category %}
        <div>
            - <a href="{{ DS.url | prepend: site.baseurl }}">{{ DS.title }}</a>
        </div>
    {% endfor %}
</div>
<div>
    <h2> DesignPattern </h2>
    {% assign DesignPattern_category = site.categories.DesignPattern | sort: 'date' %}
    {% for DP in DesignPattern_category %}
        <div>
            - <a href="{{ DP.url | prepend: site.baseurl }}">{{ DP.title }}</a>
        </div>
    {% endfor %}
</div>