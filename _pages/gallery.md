---
layout: page
title: 南北东西路
permalink: /outandabout/
description: OUT AND ABOUT
nav: false
nav_order: 4
# display_categories: [north america]
horizontal: false
---

<br/>

酝酿这个专题已有些时日，动机很纯粹，来自一首离别词的下阕：

<p style="text-align: center;">
又是离歌，一阕长亭暮。<br/>
王孙去，萋萋无数，南北东西路。
</p>

最爱最后五个字，也总想为这五个字写些什么。南北东西，四方皆是路，远行人已去，惟芳草春风吹又生。的确，从一八至二三年，年年都在告别，有时是送别好友，有时自己就是那远行人。可是，也并不太想真的写告别，只是这一次又一次的告别让我觉得，自己从出生到现在，对什么都疏于记录，去过一些地方，有过一些情绪，自己的生活也并非乏善可陈，可好像就是没有个交代，有过写作的冲动，可最后总不了了之。所以想给自己开辟一个空间，虽写不出什么鸿篇巨制，不过好歹可以给自己留下些若干年后回头看的东西。至于为何要将其公开？其实主要算是有个监督吧，可要将一些私密心思展示出来，是很需要勇气的，其中不乏一些我自己看了都觉得害羞，愿列位看官届时不要见笑。

<br/>

<!-- pages/projects.md -->
<div class="projects">
{%- if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {%- for category in page.display_categories %}
  <h2 class="category">{{ category }}</h2>
  {%- assign categorized_projects = site.projects | where: "category", category -%}
  {%- assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
  {% endfor %}

{%- else -%}
<!-- Display projects without categories -->
  {%- assign sorted_projects = site.projects | sort: "importance" -%}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
{%- endif -%}
</div>
