---
layout: page
use-site-title: true
css:
  - "/assets/css/custom.css"
---
{% assign reviewer_count = site.data.reviewers | size %}
<section class="reviewers-thanks">
  <h1>Thank You to Our Reviewers</h1>
  <p>
    The TAE workshop would not be possible without the time and expertise of our program committee.
    We are deeply grateful to the {{ reviewer_count }} reviewers listed below, who contributed thoughtful reviews
    that shaped the workshop program and helped authors strengthen their work.
  </p>
</section>

<h2 class="reviewers-heading">All Reviewers</h2>
<ul class="reviewer-list">
{% for name in site.data.reviewers %}
  <li>{{ name | escape }}</li>
{% endfor %}
</ul>
