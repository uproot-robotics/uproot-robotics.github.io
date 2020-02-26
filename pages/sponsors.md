---
layout: misc
title: Sponsors
---
{% capture htmlblock %}
    <div class="contained-img">
        {% for image in site.static_files %}
            {% if image.path contains 'img/sponsor_logos' %}
                <img src="{{ site.baseurl }}{{ image.path }}" alt="image"/>
            {% endif %}
        {% endfor %}
    </div>
{% endcapture %}
{{ htmlblock | replace: '    ', ''}}

