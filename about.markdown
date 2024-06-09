---
layout: base
title: Machine Learning Exercises
permalink: /about/
---

I am compiling a collection of exercises that I came across during my research.
Most problems have short descriptions, can be solved in a few lines, and hopefully inspires some thoughts.
<!-- Presumably, LLMs would struggle with these problems due to their originality. -->

Email me if you {want a solution, find a mistake}.
Enjoy!

{% for puzzle in site.puzzles %}
  <h3>
    <a href="{{ puzzle.url }}">
      {{ puzzle.title }}
    </a>
  </h3>
{% endfor %}