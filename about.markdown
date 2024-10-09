---
layout: base
title: Exercises
permalink: /about/
---

I am compiling a collection of exercises that I came across during my research.
Most problems have short descriptions, and can be solved in a few lines.
Some of them are original and might (hopefully) inspire some thoughts.

{% for puzzle in site.puzzles %}
  <h3>
    <a href="{{ puzzle.url }}">
      {{ puzzle.title }}
    </a>
  </h3>
{% endfor %}
