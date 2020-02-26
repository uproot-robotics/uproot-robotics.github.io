---
layout: misc
title: Sponsors
---
{% capture htmlblock %}
    <div class="contained-img">
        {% for sponsor in site.data.sponsors %}
            <a href="{{ sponsor.url }}">
                <img src="{{ sponsor.img }}" alt="image"/>
            </a>
        {% endfor %}
    </div>
{% endcapture %}
{{ htmlblock | replace: '    ', ''}}

