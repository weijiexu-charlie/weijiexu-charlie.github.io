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

<!-- <p align="justify">
所读诗词不多，却总有几句，时时念着，以下便是其一：
</p>

<p style="text-align: center;">
又是离歌，一阕长亭暮。<br/>
王孙去，萋萋无数，南北东西路。
</p>

<p align="justify">
最爱最后五字，南北东西，四方皆是路，远行人已去，惟芳草春风吹又生。想为这五个字写点什么，对于知交半零落的惆怅，却还体会不出太多，。

词中又写告别，理所应当，似乎得写告别，可自己还体会不出太多知交半零落的惆怅，对于生活，还是向前眺望的多，回首怀旧的少，若硬写告别，未免矫揉造作。不过对于每一天的日子，我的确是有些疏于记录了，于是单纯想借这五个字为题眼，为自己开辟一个空间，虽写不出什么鸿篇巨制，却好歹可以随意记录些所见所闻，所思所想。然而，将私密心思展示出来，是很需要勇气的，列位看官届时不要见笑。
</p>

<br/> -->

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

<br/>

<!-- <div class="caption">
    Thanks to Andrew for the suggestion of "Out and About" as the English translation for the title of this blog.
</div>
 -->

