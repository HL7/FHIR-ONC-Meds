# {{ page.title }} Example

<!-- source pages/\_include/{{page.md_filename}}.md  file -->

### Storyboard for this example

<!-- { { page.use_case } }-request.md -->

{% if page.use_case == 'encounter-meds' %}
{% include {{ page.use_case }}-storyboard.md %}
{% else %}
{% include all-meds-storyboard.md %}
{% endif %}


#### Request:

<!-- { { page.use_case } }-request.md -->

 {% include {{ page.use_case }}-request.md %}

#### Response:

<!-- { { page.use_case } }-response.md -->

 {% include {{ page.use_case }}-response.md %}

 <!-- { { page.use_case } }-examples.md -->

 links to example files:

  {% include meds-examples.md %}
