--- 
layout: page
title : Archive 
permalink: /archive/
hide: true
---

{% assign talks_rev=site.talks |sort: 'date' %}
{% assign talks=talks_rev | reverse %}

{% assign years=""| split: "," %}
{% assign dates=""| split: "," %}

{% for talk in talks %}
{% assign year=talk.date | date: "%Y" %}
{% assign years=years | push:year %}
{% assign date=talk.date | date:  '%s' %}
{% assign dates=dates | push: date %}
{% endfor %}

{% assign year_now=site.time | date: '%Y' %}
{% assign last_year=year_now|minus:1 %}

{% assign past_years=""| split: "," %}
{% for date in dates %}
  {% assign year=date|date:"%Y" %}
  {% assign year_int=year|plus:0 %}
  {% if year_int < last_year %}
    {% assign past_years=past_years | push: year %}
  {% endif %}
{% endfor %}

{% assign years=years|uniq %}
{% assign years_rev=years|reverse %}


{% for i in years %}
{% if past_years contains i %}
<h3>{{i}}</h3>
<table class="talks" style="overflow: hidden;">
<tbody>
{% for talk in talks %}

{% assign year=talk.date|date: "%Y" %}
{% assign year_int=year|plus:0 %}
{% capture date %}{{talk.date | date: '%s'}}{% endcapture %}

{% if year_int < last_year %}
{% if year==i %}
    {%if talk.img %}
      {% assign imgurl=talk.img %}
    {% else %}
      {% assign imgurl=site.emergency-img %}
    {%endif%}

	<tr onclick="location.href='{{ talk.url | prepend: site.baseurl }}'">
            <td><b>{{talk.speaker}}</b> <span class="affil">{% if talk.affiliation %}[{{talk.affiliation}}]{% endif %}</span><span class="event_date">{{talk.date| date: "%B %-d, %Y"}}</span><br><i>{{talk.title}}</i><br><div class="abstractbox">{{talk.content|strip_html|truncate:170}}</div></td><td></td>
        </tr>
{% endif %}
{% endif %}
{% endfor %}
</tbody>
</table>
{% endif %}
{% endfor %}
