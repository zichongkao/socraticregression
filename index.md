---
layout: default
title: Socratic Regression
---
<div class="index_content">
  <p>Socratic Regression is an attempt to develop an intuitive understanding of linear regression using Socratic questioning. The starting point for this project was a <a href="https://web.archive.org/web/20040213071852/http://emlab.berkeley.edu/GMTheorem/index.html">geometric proof of the Gauss-Markov theorem</a> which appealed to me as a visual learner. While examining it, I found myself running through lines of questioning in my head which made me realize that Socratic questioning is a more natural way to learn the subject than the traditional ground-up, proof-based approach.</p>
  <br>
  {% for tutorial in site.data.sitemap.contents %}
  <div class="index_section">
    <div class="index_section_title">
      <h3><a href="{{ tutorial.url }}">{{ tutorial.title }}</a></h3>
    </div>
    <p>{{ tutorial.blurb }}</p>
  </div>
  {% endfor %}
</div>
